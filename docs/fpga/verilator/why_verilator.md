## Simulating Verilog the Right Way

Simulating verilog in an effective manner can be a challenging task. The verilog language provides a few structures for allowing the HDL designer to describe a small testbench effectively - but is rather inadequate for complex designs. Such shortcomings become extremely painful in large complex design such as verifying and pipelining and Out of Order CPU.

## Verilator to the Rescue 

Verilator allows you to compile your Verilog source into C++ code. Suddenly, anything that you can do with C++/C, you can do with your RTL. For example, if you have RTL that has video output, you can use verilator to display that video output within your windowing system via the X Windowing System for example.

Also, the last time I checked, verilator was the fastest verilog simulator on the market.

## Simulating with Verilator
Simulating with verilator directly by writing C++ can quickly become tiring for large designs. While C++ is a much better language for describing simulations, describing multi port hardware accesses with C++ syntax is a bit unpleasant. 
To help combat this, it is possible to use verilator to simulate hardware written in [Chisel HDL] or [Spinal HDL] as verilator is integrated as a simulation backend in both Spinal and Chisel HDL. 

## Building and Running Your Testbench
While you can write your testbench in Chisel or Spinal and invoke verilator as a backend(the easier way of doing things), you may have verilog source code you have already written that you wish to test.

Dan Gisselquist has a good [tutorial] on how to use verilator. Compiling with verilator has changed a little since his tutorial was written, in particular, to build your verilog into C++, you will probably need to do the following:

    #!bash
	#this compiles your verilog into C++
	#main.cpp contains your testbench in C++
    $verilator -Wall --cc top.v submodule1.v submodule2.v submoduleN.v --exe main.cpp --Mdir /build --top-module top -Wno-fatal

where ```top``` is the name of the top module in your verilog source.
Verilator will automatically generate a makefile for you too. Invoking this makefile will build an executable that will simulate your design.
You can also have verilator spit out a VCD.
To build your executable, you do something along the lines of:

    #!bash
    $make -j -C /build -f Vtop.mk Vtop

## Example Testbench
Here is a testbench i wrote a while back for verilator. The complete source is not shown below, but just enough to give you an idea of how verilator works.

For this particular testbench, I was simulating a RISCV core that printed its status to a UART device.

### Snippet from main.cpp

    #!C++
    //here we do setup
    template <typename module>
    void testbench<module>::setup(void){
    	//setup inputs
    	m_core->clk = 0;
    	m_core->rx = 0;
    
    	m_core->eval();
    	m_core->clk = 1;
    	m_core->eval();
    	m_core->clk = 0;
    	m_core->eval();
    
    	// Tick the clock until we are done
    	int OLD_LED = m_core->led;
    	for(int i = 0; i < 500000; i++){
    		m_core->clk = 0;
    		m_core->eval();
    		m_core->clk = 1;
    		m_core->eval();
    
    		if(m_core->tx_byte_valid){
    			//printf("%s",(char)m_core->tx_byte);
    			putc(m_core->tx_byte, stdout);
    		}
    		if(m_core->led != OLD_LED)
    			printf("LED VALUE: %d\n", (char)m_core->led);
    
    		OLD_LED = m_core->led;
    	}
    }
    
    template <typename module>
    void testbench<module>::do_transmit(void){
    }
    
    void delay(testbench<Vtop> *tb){
    	for(int i = 0; i < 1250; i++)
    		tb->tick();
    }
    
    int main(int argc, char **argv){
    	// initialize verilators variables
    	Verilated::commandArgs(argc, argv);
    
    	// create an instance of our module under test
    	testbench<Vtop> *tb = new testbench<Vtop>();
    	//toggle transmit informing the UART core we are
    	//ready to transmit
    
    #ifdef TRACE_ON
    	//we've reached a condition of interest in the simulation
    	//we open the trace file to start recording
    	tb->opentrace("trace.vcd");
    	printf("tracing\n");
    #endif
    
    	printf("BEGIN SIM\n");
    	tb->setup();
    	printf("\nEND SIM\n");
    #ifdef TRACE_ON
    	tb->close();
    #endif
    }
	
	

[Spinal HDL]:https://spinalhdl.github.io/SpinalDoc-RTD/
[Chisel HDL]:https://www.chisel-lang.org
[tutorial]:https://zipcpu.com/blog/2017/06/21/looking-at-verilator.html
