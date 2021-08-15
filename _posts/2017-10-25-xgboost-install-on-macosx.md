---
layout: post
title: Installing xgboost and LightGBM on Mac OSX
tags: Dev
---

Installing xgboost and LightGBM on Mac OSX can be tedious and horrifying for someone like me who's not familiar with compilers.

## Prepare GCC

Let's start with xgboost, which is the trickier one. Run the following command:

```
brew reinstall gcc --without-multilib
```

After 30-60 mins you will have the latest version of gcc installed.

## Check GCC Availability

First, run the command:

```
gcc -v
```

The default C compiler for Mac OSX is **clang**. Normally it shows something like

```
Apple LLVM version 7.0.2 (clang-700.1.81)
```

We need to change the default C compiler from **clang** to **gcc** now. 

To check the installed **gcc** version, run the following command:

```
ls /usr/local/bin/*
```

Throughout the huge list you will find directories like:

```
/usr/local/bin/g++-7
/usr/local/bin/gcc-7
```

Remember it for future use. Now run the following command:

```
sudo vi ~/.bash_profile
```

And add the following lines to the sourcing file:

```
alias gcc='gcc-7'
alias cc='gcc-7'
alias g++='g++-7'
alias c++='c++-7'
```

Quit editing with `:wq`, run `source ~/.bash_profile` followed by `gcc -v` again, now the default C compiler should be **gcc**.

```
gcc version 7.2.0 (Homebrew GCC 7.2.0)
```

## Prepare MPI

Run `ls /usr/local/bin/*` again and try to find some directories like:

```
/usr/local/bin/mpicxx
```

If it exists, skip this part. If not, you should install the HPC library MPI instead.

[Download here](http://www.open-mpi.org/software/ompi/v1.8/downloads/openmpi-1.8.tar.gz) or check [MPI Official website](http://www.open-mpi.org/software/ompi/v1.8/) for the latest version and download the `.tar.gz` file. Run the following commands step by step:

```
tar zxvf openmpi-1.8.tar
cd openmpi-1.8
./configure --prefix=/usr/local  
make all
sudo make install
```

If you can see the abovementioned directory, then we are fine with MPI.

## Prepare xgboost

Clone xgboost from Github repo and try to compile:

```
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost
cp make/config.mk ./config.mk
make -j4
```

Now you are on the way to make a 4-core xgboost toolkit.

## Debug

If you just had success with compiling, skip this part. 

### errorunsupported option '-fopenmp'

`errorunsupported option '-fopenmp'` could still happen in rare cases even if you did switch from **clang** to **gcc**. You need to edit the `Makefile`:

```
vi Makefile
```

Look for the following lines:

```
# linux defaults
ifndef CC
export CC = gcc
endif
ifndef CXX
export CXX = g++
```

Change them to:

```
# linux defaults
ifndef CC
export CC = /usr/local/bin/gcc-7
endif
ifndef CXX
export CXX = /usr/local/bin/g++-7
```

The directories should correspond to the gcc version that you have just installed. Do the same with `make/config.mk` and try to find the following lines:

```
# choice of compiler, by default use system preference.
# export CC = gcc
# export CXX = g++
# export MPICXX = mpicxx
```

Change them to (be sure to uncomment them!!!):

```
# choice of compiler, by default use system preference.
export CC = /usr/local/bin/gcc-7
export CXX = /usr/local/bin/g++-7
export MPICXX = /usr/local/bin/mpicxx
```

This should fix the compiler bug.

### collect2: error: ld returned 1 exit status

I don't understand why this error pops up but the solution is:

```
vi Makefile
```

Search for the following line:

```
export LDFLAGS= -pthread -lm $(ADD_LDFLAGS) $(DMLC_LDFLAGS) $(PLUGIN_LDFLAGS)
```

Now add a link before it:

```
export LDFLAGS= -pthread -lm -mmacosx-version-min=10.9
```

Quit with `:wq`, clean up with `make clean`, then do `make -j4`. No errors should be raised anymore.

## Install Python library

To run xgboost with Python, you need to install the library:
```
cd python-package
sudo python setup.py install
```
Try `import xgboost` in Python notebook and it should be fine.

## Install LightGBM

After all what we have done for xgboost it's effortless now to work on LightGBM. The only thing we need to prepare is **cmake**.

```
brew install cmake
```

The following steps are pretty much generic.

```
git clone --recursive https://github.com/Microsoft/LightGBM
cd LightGBM
export CXX=g++-7 CC=gcc-7
mkdir build
cd build
cmake ..
make -j4
```

LightGBM is available through `pip` for python:

```
pip install --no-binary :all: lightgbm
```

## Reference

- [Prepare GCC](http://blog.csdn.net/u010167269/article/details/51951582)
- [Prepare MPI](http://blog.csdn.net/u014247371/article/details/26089411)
- [General procedure](http://www.cnblogs.com/akanecode/p/7708047.html)
- [Debugging](https://github.com/dmlc/xgboost/issues/261)
- [LightGBM](https://lightgbm.readthedocs.io/en/latest/Installation-Guide.html)
