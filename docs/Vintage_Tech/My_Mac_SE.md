# My MacSE 
![](Mac_SE.jpg)
This MacSE is mine! It was graciously given to me by the youth pastor at my church. As I mentioned earlier, it is in perfect working order. So I plan to make some serious modifications to make it a more compelling computer in the 21st century. This computer has a fairly simple processor with no MMU, so it should be fairly easy to write software and install my own accelerators onto its bus slot in the back. Apple provided fairly good documentation for this machine and as far as I can tell - it should be fairly straight forward to write drivers for this machine.

## Writing Software 
Thanks to [Retro68] for making a complete cross compiler toolchain that even works with MacSE emulators. This way, I can write and test all software while never leaving my Macbook Pro!

The Retro68 toolchain is built on top of GCC and also has a framework that can generate graphical applications for Classic Mac.

## Interesting Ideas 
### Custom ADB Mouse 
My youth pastor forgot or lost his mouse - so when I first got the MacSE, I had no mouse. I had a spare ADB mouse at school which I picked up later - but became seriously fascinated with the ADB protocol in the process - so one thing I would like to do is build my own ADB compliant mouse.

### FPGA Graphical Accelerator with Accompanying Drivers 
The MacSE also comes with an PDS expansion slot. The SE's system architecture is so simple that it should be possible to design an FPGA graphical accelerator and write a driver to interface with it. This might be useful for writing a rendering engine - something the MacSE could never do otherwise. The graphical accelerator could perhaps even be used to decode videos.

### Parallel Dot-Matrix Printer Driver 
My friend Michael Nolan recently ordered a parallel dot-matrix printer and wrote some graphical dithering algorithm to support printing pictures. It would be interesting to package these algorithms and accompanying fonts into a driver allowing my MacSE to print documents. 

The MacSE lacks a parallel port, so making the appropriate RS422 serial to parallel convertor would probably also be part of the project.

### SD to SCSI card Convertor 
Now I know that this has already been done, but these are expensive($75). I don't want to spend more than $50 total playing with this MacSE - so I opt to make my own. It should be perfectly possible to do this with a decently fast Arduino as the MacSE CPU is only clocked at 8MHZ.

[Retro68]: https://github.com/autc04/Retro68
