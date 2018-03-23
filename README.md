# AICS JXRlib repo

## What this project is 
This is a fork of https://github.com/glencoesoftware/jxrlib.
The intent of this fork is 2 fold. First to incorporate library
changes from [Christoph Gohlke.](https://www.lfd.uci.edu/~gohlke/code/jxrlib_CreateDecoderFromBytes.diff.html)
The second reason is to trim down the original JXRlib project to remove building 
the Java components of the library to excise the JDK requirements in an 
attempt to make the package more easily installable. The intent is that 
this will be part of a pip installable package. 

It will create: 
libjpegxr.a
libjxr++.a 
libjxrglue.a

### CMake not Make
For compactness and clarity we created a CMakeList.txt in a hope to make this 
a lot easier to build. It is also hoped that if this package needs to be pulled 
in as a submodule that CMake will automatically build this project without any 
headaches.


## What this does not provide
This variant of the JXRlib package is solely targeted 
at [CziFile](https://www.lfd.uci.edu/~gohlke/code/czifile.py.html) support
it does not build the swig target, any java version of the library, nor either 
of the executables built by the original package (JxrEncApp or JxrDecApp).

## How to Install
1) clone the repo
2) mkdir build
3) cd build
4) cmake ../
5) make 
6) make install