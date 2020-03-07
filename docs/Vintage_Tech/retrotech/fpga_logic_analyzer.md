Due to the impracticality of using DDL's LA(logic analyzer) and the difficulty I encountered using the LA's interface, I decided to implement an LA on my own FPGA. Below are some prerequisites about for such an approach.

 - FPGA has 3.3V logic levels
 - Proficiency in VHDL/Verilog 
 - Proficiency with Python or some frontend language 
 - Proficiency with bash

I found a Logic Analyzer project by the name of [SUMP2] written and maintained by Kevin Hubbard from Washington, USA.
After a few emails with Kevin Hubbard. I was able to get the RTL LA backend on my HX8k FPGA and the Python frontend working. I document this process below.

## Python Frontend Viewer 

The [repository here] contains the RTL modified Python frontend as described below. There you will find installation instructions and configuration specifics for running SUMP2 on modern Macs. Here are a couple notes about changes I made.

Added "Darwin" to supported OS system - leaving system call behavior unchanged as MacOS terminal calls rarely deviate from those of Linux
  
    #!python
    if  self.os_sys != "Linux" and  self.os_sys != "Darwin":

And again on lines 2028, 2039, 2050, and 2178

    #!python
    if ( self.os_sys == "Linux" or self.os_sys == "Darwin"):


I also changed the default font for MacOS system to make it more readable - pygame doesn't seem to play nice with non-retina display

    #!python
    if(self.os_sys == "Darwin"):
        font_height = int( font_height, 20 ); # Conv String to Int
    else:
        font_height = int( font_height, 10 ); # Conv String to Int


## Verilog Backend and Sump2 Protocol 
It was necessary to make a number of modifications to the verilog to get it working with the Lattice HX8K. I originally used the proprietary ICEcube2 software to synthesize, time, layout, and verify my designs, but I quickly switched to the  [IceStorm] toolchain by Clifford Wolf as it is much faster and reliable.

## Assigning Pinouts 
The sump2 verilog was designed for the Lattice HX1K icesticick board, so I first went through the [HX1K's schematics] while browsing through top.v. I had to hook up the FTDI ports to their respective pins on the HX8k. To do this, I referenced the HX8K [schematics]. I also assigned the status LEDs to their respective pins.

| set_io | clk_12m  | J3  |
|--------|----------|-----|
| set_io | ftdi_wi  | B10 |
| set_io | ftdi_ro  | B12 |
|        |          |     |
| set_io | spi_sck  | R11 |
| set_io | spi_cs_l | R12 |
| set_io | spi_mosi | P11 |
| set_io | spi_miso | P12 |
|        |          |     |
| set_io | LED1 B5  |     |
| set_io | LED2 B4  |     |
| set_io | LED3 A2  |     |
| set_io | LED4 A1  |     |
| set_io | LED5 C5  |     |
| set_io | LED6 C4  |     |
| set_io | LED7 B3  |     |
| set_io | LED8 C3  |     |

[SUMP2]:https://blackmesalabs.wordpress.com/2016/10/24/sump2-96-msps-logic-analyzer-for-22/
[schematics]:http://www.latticesemi.com/-/media/LatticeSemi/Documents/UserManuals/EI/EB85.ashx?document_id=50373|schematics
[HX1K's schematics]:http://www.latticesemi.com/~/media/LatticeSemi/Documents/UserManuals/EI/icestickusermanual.pdf
[edits here]:https://github.com/BracketMaster/SUMP2_4_UNIX
[IceStorm]:http://www.clifford.at/icestorm/
