# Running SCHISM for the first time
1 Check Windows in Command prompt

  Press `Win + R`, type cmd, and press Enter, or search for `Command Prompt` in the Start menu

  `winver`

  This will open a window displaying your Windows version and build information

2 Install Ubuntu linux on Windows
  
  Click: https://www.youtube.com/watch?v=wjbbl0TTMeo&pp=ygUjaG93IHRvIGluc3RhbGwgdWJ1bnR1IG9uIHdpbmRvd3MgMTE%3D
  
3 Ubuntu installed

  To navigate to the C:\schism directory from within Ubuntu (assuming it’s mounted in the /mnt directory), you can use the following command
 
  `cd /mnt/c/schism`

# Error: compiler mpif90

Install MPI. You can choose between openmpi or mpich, depending on your preference. To install openmpi, run: 
   
    `sudo apt update`, then
  
    `sudo apt install openmpi-bin openmpi-common libopenmpi-dev`

# No python

1. Install Python or link it correctly. First, check if Python 3 is installed

  `python3 --version`

2. If Python 3 is installed, create a symlink so python points to python3
 
  `sudo ln -s /usr/bin/python3 /usr/bin/python`

3. If Python is not installed, install it with:
 
  `sudo apt update`
 
  `sudo apt install python3`

# Error: Makefile:115: o/svn.BORA/autosrc.mk: No such file or directory

This error means autosrc.mk is missing.

Check if there’s an instruction to generate this file, or try running:

`make clean`

# Error after type: make pschism

1. /bin/sh: 1: ../mk/sfmakedepend.pl: not found

Check if sfmakedepend.pl exists in your project. If not, it may have been missed during the project setup or download. Ensure that all files are fully downloaded or cloned from the repository.

If sfmakedepend.pl exists but is located elsewhere, you might need to modify the Makefile to point to its correct path.

2. Perl Dependency

Install Perl if it’s missing:

`sudo apt update`

`sudo apt install perl`

3. Reset building environment

`make clean`

`make pschism`

# To build the SCHISM code using CMake 

1. Navigate to the cmake/ Directory

`cd /path/to/schism/cmake`

2. Set Up SCHISM.local.build

`TVD_LIM=1`

`BLD_STANDALONE=ON`

`NO_PARMETIS=ON`    # (if bypassing ParMETIS)

`OLDIO=ON`          # (set to OFF if you want to use asynchronous I/O)

3. Set Up SCHISM.local.cluster_name

`cp -L SCHISM.local.whirlwind SCHISM.local.myown`

4. Run CMake

`cd ..`

`mkdir build`

`cd build`

5. Run cmake to configure the build:

`cmake ..`

This will read the CMake files and configure the build based on the settings you specified in the SCHISM.local.build and SCHISM.local.cluster_name files.

6. Build the Code

Compile the project by running make within the build directory:

`make -j$(nproc) pschism`

# Error: I cannot find the file build/ CMakeCache.txt
The cmake is essentially a pre-processor for make, and it creates cache files (e.g. build/ CMakeCache.txt, where you can inspect all env variables). After cmake is done, make can be executed in parallel or in serial mode.

If the CMakeCache.txt file is missing, it usually indicates an issue during the CMake configuration process.

1. Remove Old build/ Directory and Retry

  `cd ../build`

  `rm -rf *`
2. Run CMake with Verbose Output
 
  `cd ../build`

  `cmake -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/ -DCMAKE_VERBOSE_MAKEFILE=ON`

# Error: Compiler not found

It is difficult for me to install ifort, so I use gfortran

Download OneAPI Compiler first

Code to try ifort (but turn out I cannot get it)

`nano ~/.bashrc`

1. Ensure mpif90 uses Intel Fortran compiler instead of Gfortran

`export I_MPI_F90=ifort`

2. Source the setvars.sh script to set up paths and environment variables

3. Replace /opt/intel/oneapi with the actual path where Intel oneAPI is installed

`source /opt/intel/oneapi/setvars.sh > /dev/null`

`source ~/.bashrc`

`mpif90 -show`

`ifort --version`

# I CANNOT FIND IFORT AGAIN
1. Alternative: GNU Fortran

`sudo apt update`

`sudo apt install gfortran`

2. After installed, build SCHISM

`/mnt/c/schism/build`

`cd /mnt/c/schism/build`

`ls`

# Error: Cmake Fortran compiler

Verify, `gfortran --version`

Set gfortran as the Fortran Compiler in CMake

`cd /path/to/your/build/directory`

`cmake -DCMAKE_Fortran_COMPILER=gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

`make -j$(nproc)`

# Build directory

`ls /mnt/c/schism`

`cd /mnt/c/schism/build`

# Run CMake with gfortran

`cmake -DCMAKE_Fortran_COMPILER=gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

`cd /mnt/c/schism`

`mkdir build`

`cd build`

# Error again

`which gfortran`

`cmake -DCMAKE_Fortran_COMPILER=/usr/bin/gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

`cmake -DCMAKE_Fortran_COMPILER=gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

`export FC=gfortran`

`cmake -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

# Error: NET CDF
1. Install NetCDF Libraries

`sudo apt update`

`sudo apt install libnetcdf-dev libnetcdff-dev`

2. Verify Installation of nc-config

`nc-config --version`

3. Set NetCDF Paths Explicitly in CMake

`nc-config --cflags`    # This will give the include directory path

`nc-config --libs`      # This will give the library path

`cmake -DNetCDF_C_LIBRARY=/usr/lib/x86_64-linux-gnu/libnetcdf.so \
      -DNetCDF_Fortran_LIBRARY=/usr/lib/x86_64-linux-gnu/libnetcdff.so \
      -DNetCDF_INCLUDE_DIR=/usr/include \
      -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

4. Clear CMake Cache and Retry

`rm -rf CMakeCache.txt CMakeFiles/`

# Still error

1. Install g++ (GNU C++ Compiler)

`sudo apt update`

`sudo apt install build-essential`

2. Verify the Installation

`g++ --version`

3. Set CXX Environment Variable

`export CXX=/usr/bin/g++`

4. Clear CMake Cache

`rm -rf CMakeCache.txt CMakeFiles/`

# Re-run

`cd /path/to/your/build/directory`

`rm -rf CMakeCache.txt CMakeFiles/`

`cmake -DCMAKE_CXX_COMPILER=/usr/bin/g++ -DCMAKE_Fortran_COMPILER=gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

`make -j$(nproc)`

# Error compiler

`gfortran --version`

`sudo apt update`

`sudo apt install gfortran`

`which gfortran`

`cmake -DCMAKE_Fortran_COMPILER=/usr/bin/gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

`rm -rf CMakeCache.txt CMakeFiles/`

# Run the CMake configuration and build your project using make on Ubuntu. Replace paths and options as needed.

1. Navigate to the Build Directory and Run CMake

2. Navigate to the build directory (create if it doesn't exist)

`cd /path/to/your/project/build || mkdir -p /path/to/your/project/build && cd /path/to/your/project/build`

3. Run CMake configuration

`cmake -DCMAKE_Fortran_COMPILER=gfortran -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.myown ../src/`

4. Compile the Project with make

`make -j8 pschism`

`make VERBOSE=1 pschism`

# Execute SCHISM sample run test

1. Ensure svn is Installed

`sudo apt update`

`sudo apt install subversion`

2. Download SCHISM Verification Tests Using svn

`svn co https://columbia.vims.edu/schism/schism_verification_tests`

3. Locate and Use the Correct param.nml File

4. Replace /path/to/schism with the actual path where SCHISM is installed

`cp /path/to/schism/sample_inputs/param.nml /path/to/schism_verification_tests/`

5. Check for Updates in Readme.beta_notes

`cat /path/to/schism/src/Readme.beta_notes`

# Run the Benchmark Tests

`cd schism_verification_tests`

# Explore the available test cases

`ls`

Last edited 10 November 2024
