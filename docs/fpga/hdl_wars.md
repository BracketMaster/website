# Verilog, VHDL, Chisel, Spinal, Migen, PyMtl and others

There are many HDLs out there, but the two most important ones are Verilog and VHDL. These are supported by all ASCI design suites and FPGA synthesizer tools(some tools also support the synthesizeable subset of SystemVerilog).

## Some Analogies
If boolean logic gates were assembly, then Verilog and VHDL would be like assembly with macros. PyMtl and SystemVerilog, would be like C. Chisel, SpinalHDL, Migen, nMigen would be C++ or perhaps Python. This is not an entirely accurate comparison(PyMtl and SystemVerilog are somewhat focused on delivering different levels of simulation fidelity depending on the needs), but provide an illustration of sorts.

## Problems with Verilog and VHDL
The main shortcomings of Verilog and VHDL is that they lack any modern software language features, namely: OOP. 
The other languages listed are not supported by ASIC or FPGA tools, so this means these languages must compile down to Verilog or VHDL. Migen and PyMtl I believe compile down to Verilog only.

## What's Used in Industry
| HDL            | Used by                                                                                    |
|----------------|--------------------------------------------------------------------------------------------|
| Verilog        | Everybody at some point in the design process(for those who don’t use VHDL)                |
| VHDL           | Mostly government - makes sense, VHDL was commissioned by DARPA                            |
| System Verilog | Common in industry for ASIC verification                                                   |
| Chisel         | Google in their TPU, Berkeley Architecture Research(Invented here), popular in open source |
| SpinalHDL      | Fork of ChiselV2, functionally similar to ChisalV3, popular in open source                 |
| Migen          | Some European companies                                                                    |
| nMigen         | Brand new child of Migen. Not used much yet.                                               |
| BlueSpec HDL   | router design in industry                                                                  |
| PyMtl          | Mostly research - Cornell designed Celerity ASIC with it                                   |

## What I might use
In the following pages, I discuss some strengths of migen and verilator(a verilog simulator). My general approach is to avoid Verilog and VHDL where possible as they are rather unpleasant for large designs. 

Now that the verilator backend for nMigen is nearly complete,
I try to avoid Chisel and its Spinal fork for the following reasons.

1. Chisel has excessive simulator boilerplate and no easy simulation support for multiple clock domains.
2. Scala also has really slow SBT compile times and often requires doing the SBT package manager dance - 
contrast with Python's Pip which just works.
3. I also find Python idiosms much more simple/useful/natural than Scala's

I would also avoid VHDL altogether. VHDL has no fast simulators available.

