# Dakota
Python-interfaced Dakota inputs for optimisation

This repository is a reference for interfacing Dakota with a python code for optimisation.Three different optimisation methods are chosen as examples:
1. a gradient-based local optimisation: Quasi Newton Method
2. a derivative-free local optimisation: Pattern Search Method
3. a derivative-free global optimisation: Genetic algorithm

Each of the optimisation can be run with a command 'dakota -i *.in' after installation of Dakota.

# A guidance for installing Dakota with compiling from source(in Ubuntu system)

1. Preparation:

1.1 Install necessary packages

sudo apt-get install gcc g++ gfortran cmake cmake-curses-gui libboost-dev libboost-all-dev libblas-dev liblapack-dev libopenmpi-dev openmpi-bin openmpi-doc xorg-dev libmotif-dev

Note: Installation of GNU, CMake, Boost, libblas, liblapack, Openmpi, xwindows, Python

1.2 Download DAKOTA source code on website

cd ~
mkdir DAKOTA
cd DAKOTA
wget https://dakota.sandia.gov/sites/default/files/distributions/public/dakota-6.10-release-public.src.tar.gz
tar -xzf dakota-6.10-release-public.src.tar.gz 

1.3 create the build and installation folder

mkdir dakota_build
mkdir dakota_installation
export DAK_BUILD=$HOME/DAKOTA/dakota_build
export DAK_INSTALL=$HOME/DAKOTA/dakota_installation
export DAK_SRC=$HOME/DAKOTA/dakota-6.10.0.src
cd $DAK_BUILD 

**if no parallel computing is needed, skip step 1.4 and 1.5.

1.4 Copy file BuildDakotaTemplate.cmake and rename it to BuildDakotaCustom.cmake. You can edit some features if you want, check the content inside for details.
(Note: the modification will be based on BuildDakotaCustom.cmake, so the source code won't be damaged)
cp $DAK_SRC/cmake/BuildDakotaTemplate.cmake $DAK_SRC/cmake/BuildDakotaCustom.cmake 

1.5 Setting the mpi path to Dakota. The default setting is to turn off the mpi link, which is very annoying!!!!!
Open the file BuildDakotaCustom.cmake from DAK_SRC/cmake
Add the following lines to BuildDakotaCustom.cmake:

set( DAKOTA_HAVE_MPI ON 
     CACHE BOOL "Always build with MPI enabled" FORCE)
set( MPI_CXX_INCLUDE_PATH
     "/path/to/MPI/include"
     CACHE FILEPATH "Use installed MPI headers" FORCE)

"/path/to/MPI/include" can be found with command: mpicxx --showme, for example, on my Desktop, the path is "/usr/lib/x86_64-linux-gnu/openmpi/include"

2. Building

2.1 Run Cmake

ccmake -C $DAK_SRC/cmake/BuildDakotaCustom.cmake $DAK_SRC -DCMAKE_INSTALL_PREFIX=$DAK_INSTALL 

Note:
Cmake shows a simply GUI and when you enter in ccmake, first press the key “c” to configure the script.
If any error appears, go to the last line to check. If no, press “e” to return the main GUI.
In main GUI, press “t” to toggle some features. Now you can move down and up the cursor. You can turn ON and OFF the features by press Enter.
All finish, press “c” again to configure all scripts. If no error appears, you can press “e” to back to the main menu.
Finally press “g” to start generating the configuration.

2.2 Run make

make -j4
Note: that's to compile in parallel with 4 processors
make install

3. Path and Test

3.1 Add path

gedit ~/.bashrc

Note: add the following lines to .bashrc
export PATH=$PATH:~/DAKOTA/dakota_installation/bin:~/DAKOTA/dakota_installation/test
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:~/DAKOTA/dakota_installation/bin:~/DAKOTA/dakota_installation/lib
export PYTHONPATH=$PYTHONPATH:~/DAKOTA/dakota_installation/share/dakota/Python

3.2 Test build

dakota -v
If the screen show the version and build date, the job is finished!

Reference for installation: https://chienfm.com/dakota/Compile-Dakota-for-Ubuntu-16.04/
