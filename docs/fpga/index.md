# Getting Started
There are a few ways to get started with programming FPGAs. Programming FPGAs doesn't have to be expensive.
There are a few open-source and open-hardware FPGA design options out there which will be considered here.
Since I mostly do my programming on MacOS, this tutorial will reflect that. You can use the appropiate package manager commands for your platform.

# Toolchains
Writing hardware for FPGAs is most easily facilitated with a workflow. Mine usually consist of an HDL simulator, an HDL synthesizer, and an FPGA specific toolchain. While many of the vendor specific FPGA softwares will provide an all in one solution, I usually find them rather clunky and slow. In addition, licenses are very expensive, which means that anyone lacking a license cannot contribute to or modify any of my designs.

To solve this, I turn to open source. Lattice's entire ICE40 FPGA line, along with most of their ECP5 FPGA device line have been fully reverse engineered boasting up to 8000 and 90,000 LUTS respectively. The open source toolchain for the ICE40 is called ICESTORM, the open source toolchain for the ECP5 is called prjtrellis. Since prjtrellis proved particularly difficult to install, I will document that process here.
## Perequisites
Visit [software setup](/my_setup/software) and make sure your system has a matching configuration.
You will also need to install

    #!bash
    $brew install qt5
    $brew install boost
    $brew install boost-python3

## PrjTrellis 

Since I use a Mac, my tutorial will be focused on getting [PrjTrellis] up and running on MacOS and should be somewhat translatable to similar UNIX platforms. You'll want to clone the prjtrellis directory and follow the instructions in the README. I use brew to build all my packages. 
### Steps 
  - A few things to watch out for that could trip out your installation.
  - If you choose /usr as your install prefix, you may have to disable MacOS SIP. To get around this, just set /usr/local as your installation prefix.
  - Make sure you can execute ''$python3''

  - mkdir build
  - ```cmake -DCMAKE_INSTALL_PREFIX=/usr/local .```
  - make
  - make install
## NextPNR 
```
cmake ../ -DARCH=ecp5 DTRELLIS_ROOT=/path/to/cloned/prjtrellis
make -j($nproc)
```

[PrjTrellis]: https://github.com/SymbiFlow/prjtrellis
