## Introduction

Migen is a high level hardware description language(not to be confused with HLS - high level synthesis language).
Much like Chisel and SpinalHDL, Migen allows the hardware designer to use modern language features such as OOP to describe hardware. 
Instantiating a bus between two devices in hardware can be as simple as passign the device to an instantiation of the bus class. Whereas Chisel and Spinal are both written in Scala, Migen is written in Python3.

### Advantages
 - The primary advantage Migen offers is SOC design. Many complete HDL device libraries are availble and written in Migen. 
 Some devices include, Ethernet with ARP/UDP, HDMI, and DRAM controller just to name a few. 
 - Migen+LiteX = builtin support for common Xilinx FPGAs as well as the FOSS toolchains such as yosys+nextpnr for Lattice FPGAs. Instantiating Ethernet on the ECP5 FPGA can be a simple as importing LiteX and migen and then calling ```build()``` which will generate the bitstream for the repsective ECP5.
 - All the Python perks are availabe such as dictionaries, lists, list comprehension, etc.
 - Migen is Python, and Python code just looks clean.

### Disadvantages
 - The primary disadvantage of Migen is that simulation is slow. Also, since Python3 never targeted multithreaded support, it is basically impossible to simulate Migen on more than one core.

The way I might use Migen is to add SOC support after I've written a component such as a CPU or ML accelerator in SpinalHDL or Chisel. Migen makes it easy to add and subsequently simulate HDMI video output(for example) to your already existing project. For design components that are simulation intensive, I would use Chisel or Spinal.

A new iteration of Migen called nMigen attempts to solve Migen's shortcomings. In particular, nMigen allows generation of modular verilog code and should in the future add verilator backend support. Upon verilator integration, I would recommend nMigen for simulation intense component design.

