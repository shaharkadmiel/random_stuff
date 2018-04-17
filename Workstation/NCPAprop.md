Compiling NCPAprop on macOS:

1. Clone the github repo:
```shell
git clone git@github.com:chetzer-ncpa/ncpaprop.git
```

2. Make sure you have all pre-requisites. If not:
```shell
brew install gcc fftw gsl
```
* [Homebrew](https://brew.sh/) is the goto package manager for macOS these days.

3. Configure for installation:
```shell
cd ncpaprop

./configure --disable-library-guess \
  CXX=g++ CC=gcc FC=gfortran
```

4. Patch `PETSc` and `SLEPc` `makefiles`. Running `make` at this point ended in the following error:
```
make[2]: *** No rule to make target `/Users/shahar/tools/ncpaprop/src/extern/petsc-3.2-p5//arch-darwin16.7.0-c-debug/conf/petscrules'.  Stop.
make[1]: *** [.petsc-real] Error 2
make: *** [extern] Error 2
```

which I traced back to:
```
...
cd petsc-3.2-p5 ; \
	./configure PETSC_ARCH=arch-darwin16.7.0-c-debug --with-cc=/usr/local/bin/gcc-7 --with-fc=/usr/local/bin/gfortran-7 --download-f-blas-lapack --with-shared-libraries=0 --with-mpi=0 ; \
	cd ..
Configure does not support Python 3 yet, please run as
  python2 './configure' ...
```

so I edited the `src/extern/Makefile` to replace `./configure` with `python2.7 ./configure`:
```shell
sed -i '' 's|./configure|python2.7 ./configure|g' src/extern/Makefile
```

5. More patching... `make` resulted in more errors. Looking through the output I found several things:

`FBLASLAPACK` compilation gave this message:
```
===============================================================================
    It appears you do not have valgrind installed on your system.
    We HIGHLY recommend you install it from www.valgrind.org
    Or install valgrind-devel or equivalent using your package manager.
    Then rerun ./configure
===============================================================================
```
I installed it with:
```shell
brew install valgrind
```

I commented out all the `make ... test` lines in `src/extern/Makefile`:
```
sed -i '' '/test/s/^/#/g' src/extern/Makefile
```

and tried again:
```
make clean && make
```

5. `make` -- This takes a long time... *wait for it*
