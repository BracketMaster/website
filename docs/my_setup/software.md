# Software Setup 
Sometimes I find myself forgetting how I configured and built particular software. This page is to remind myself of my base configuration and perhaps help others with similar issues.

## Package Manager and Environment 
I usually use the brew package manager due to its apparent and rather surprising robustness. I use MacPorts on my vintage Macs such as my iMac G3 since its PowerPC packages are still maintained. But brew seems to be far less invasive than MacPorts, going so far as to not require administrative privileges to install packages. Whatever package manager is used, I suggest being consistent, brew built software tends to easily find other brew packages - same with MacPorts. It is difficult to mix and match packages from different package managers.

## Various Packages 
### GNU 
Since this is the 21st and GNU really is the UNIX standard somewhat ironically - it is important to replace the deprecated BSD utils with their respective GNU coreutils. For example, ''sed'' becomes ''gsed'', ''readlink'' becomes ''greadlink'' and so on. Just do ```brew install coreutils```

Also, if you want to do any sort of useful/common software development, you'll want to install the following packages.
### Common 

```bash
$brew install cmake
$brew install python3
$brew install pkg-config
$brew install ctags
```

### Mathematics 
If you like to do mathematics(which there is really no reason you shouldn't), you have a couple of good options when it comes to open source.

 - The Python route
 - The Maxima route

!!! note
    Technically this is not true. For those of you who like to reason about abstract algebras
	or work with the infinite, there are software like CoCoa (see [Wikipedia]).

I know there are other options that are big in research such as Julia(which i found to be rather brittle in my experience). But I've found the python route to be sufficient for my needs as it provides calculus operators, vector calculus operators, common linear algebra operators, and symbolic and or numerical manipulation for all the above. Sympy handles symbolics and integrates nicely with numpy for numerics. Sympy also generates LaTeX output which Jupyter readily consumes.

#### Python Route

!!! warning
	If you're not familiar with the python language, 
	it is much easier to get started with WxMaxima.
	But I've found mathematics with python to be far more 
	flexible when it comes to more complex operations such as 
	plotting the gradient of an electromagnetic field.

```bash
$pip3 install sympy numpy scipy jupyterlab
```

#### Maxima Route

    #!bash
    $brew install maxima
    $brew install wxmaxima
    $pip3 install --user numpy scipy matplotlib ipython jupyter pandas sympy nose

### FPGA Dev 

    #!bash
    brew install yosys
    brew install icarus-verilog
    brew install verilator

### Embedded Dev 
In case you have an MBED or Arduino lying around and don't already have enough troubles as it is.

    #!bash
    $brew tap ArmMbed/homebrew-formulae
    $brew install arm-none-eabi-gcc
    $brew tap osx-cross/avr
    $brew install avr-libc

### GUI Dev 
WxPython
On MacOS - it's pretty simple

```bash
brew install wxPython
```

Ubuntu - it's not so simple

replace ```18.04``` with your ubuntu version which you can determine by invoking ```lsb_release -a```

If you tried installing wxPython with apt - you may notice its broken. 

You might have to remove the wx.py in your home directory. ```rm ~/wx.py```

```bash
pip3 install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04 wxPython
```

Verify your wxPython was correctly installed by running the wxPython demo.

    #!bash
    $git https://github.com/wxWidgets/Phoenix.git
    $cd Phoenix/demo
    $python3 demo.py

[Wikipedia]: https://en.wikipedia.org/wiki/List_of_computer_algebra_systems
