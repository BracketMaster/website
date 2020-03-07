# Adding Verilator as a nMigen-HDL Python Simulator Backend

Recently, I've been using Migen and nMigen to build hardware for 
both Nerual Network accelerators and toy GPUs.
HDL simulation in both nMigen and Migen currently occurs in 
pure Python, which is way too slow for simulating operations of 
high arithmetic intensity such as matrix multiplies.

Verilator is the fastest verilog simulator on the market and is
also open-source, making it a perfect candidate for 
an Migen or nMigen backend. I was originally considering
 implmenting the verilator backend for Migen, but since 
nMigen improves on the mistakes of Migen(especially in the 
of clearly defining what is idiomatic nMigen and what is not,
I decided to implment the verilator backend for nMigen.

One particular advantage of nMigen over Migen is that
it retains the hierarchical structure of the HDL AST when 
emitting verilog. In simulation, nMigen exposes all ports
of the top module and submodules. Verilator however 
does not natively expose its the internals of submodules.

Rolling the verilator backend required the following steps:

1. Compile nMigen top module into verilog
2. Export the relevant ports that the default nMigen 
simulator would have access too.
3. Generate a wrapper module in verilog that declares
DPI setter and getter methods for the nMigen signals of
interest.
4. Verilate verilog model and compile verilated into a 
shared object loaded by Python.

It took quite a while for me to figure out how to implment the
DPI methods, so I have pasted what I did below.

More information on this [here](https://github.com/verilator/verilator/issues/2071).

## Implmenting DPI

```bash
rm -rf obj_dir/
verilator --cc --exe test_wrap.v test.v test.cpp
make -C obj_dir/ -f Vtest_wrap.mk
./obj_dir/Vtest_wrap 
```

### test.v
```verilog
module t (input [71:0] a, output [71:0] o);
   adder add (.a(a), .b(), .o(o));
endmodule

module adder (input [71:0] a, input [71:0] b, output [71:0] o);
   assign o = a + b;
endmodule

```

### test_wrap.cpp
```verilog
module t_wrap (input [71:0] a, output [71:0] o);
   t dut (.a(a), .o(o));

   export "DPI-C" function read_a;
   function void read_a(output bit [71:0] result);
       result = dut.add.a;
   endfunction

   export "DPI-C" function write_a;
   function void write_a(input bit [71:0] in);
       dut.add.a = in;
   endfunction

endmodule
```

### test.v
```c++
#include "Vtest_wrap.h"
#include <cstdio>

extern void write_b(char);

void print_by_int(uint32_t* val){
	printf("print_by_int\n");
	for(int i = 0; i < 3; i++)
		{
		printf("%d: ",i);
		printf("%2x\n",val[i]);
		}
}

int main(int argc, char** argv) {
    Vtest_wrap top;

    svSetScope(svGetScopeFromName("TOP.t_wrap"));

	//set and read top.a using verilator public method
	top.a[0] = 1;
	top.a[1] = 2;
	top.a[2] = 3;
	top.eval();
	printf("from verilator ");
	print_by_int(top.a);

	//set top.a using DPI and read with DPI
	uint32_t ret[] = {0,0,128};
	top.write_a((svBitVecVal *)&ret);
	printf("from DPI ");
	print_by_int(ret);

	//read top.a with verilator public method - should
	//match output of DPI
	printf("from verilator ");
	print_by_int(top.a);

	top.final();
	return 0;
}
```

### output
```
from verilator print_by_int
0:  1
1:  2
2:  3
from DPI print_by_int
0:  0
1:  0
2: 80
from verilator print_by_int
0:  0
1:  0
2: 80
```
