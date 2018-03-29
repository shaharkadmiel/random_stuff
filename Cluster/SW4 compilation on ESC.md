Installing GCC 7.3:

as root:

```shell
cd /share/apps/src
wget -c http://ftp.snt.utwente.nl/pub/software/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz
tar -xf gcc-7.3.0.tar.gz
cd gcc-7.3.0
./contrib/download_prerequisites

cd ..
mkdir temp
cd temp
/share/apps/src/gcc-7.3.0/configure --prefix=/share/apps --disable-multilib
make  # This step takes  a l o n g  time, you can speed it up with -j 10
make install
rm -rf *
```

Installing Proj4 5.0.0:

as root:

```shell
cd /share/apps/src
wget -c http://download.osgeo.org/proj/proj-5.0.0.tar.gz
tar -xf proj-5.0.0

cd ..
mkdir temp
cd temp
/share/apps/src/proj-5.0.0/configure --prefix=/share/apps
make install

cd ..
rm -rf temp
```

Compiling SW4:

```shell
cd /share/apps/src
wget -c https://geodynamics.org/cig/software/github/sw4/v2.01/sw4-v2.01.tgz
tar -xf sw4-v2.01.tgz
cd sw4-v2.01

echo "
proj = yes

FC = /opt/mpich2/gnu/bin/mpif77
CXX = /opt/mpich2/gnu/bin/mpicxx
EXTRA_LINK_FLAGS = -L/usr/lib64 -llapack -lblas -Wl,-rpath=/share/apps/lib/../lib64 -lgfortran -Wl,-rpath=/share/apps/lib -lproj
" > make.inc

make

ln -s /share/apps/src/sw4-v2.01/optimize/sw4 /share/apps/bin/sw4

```