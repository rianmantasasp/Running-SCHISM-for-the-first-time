Steps for compiling SCHISM using Cmake approach on WSL — Working notes
This guide aims to set-up SCHISM on a local windows machine to allow quick testing of models using a Windows Subsystem for Linux (WSL) distribution prior to sending models to the UQ HPC for full simulation runs.
Follows instructions in online SCHISM manual. Assumes that a copy of the source code has been downloaded from the SCHISM GitHub site.

# Install WSL
Open a microsoft command prompt
Check if a WSL distribution is already installed: wsl --list --all
(If you are using an existing distribution, you can keep using it.)
(If you want to remove an existing distribution: wsl --unregister <DistributionName>)
Check available distributions with command wsl.exe –list --online
Choose a distribution and install it: wsl.exe –install <DistributionName>
(I selected: wsl --install Ubuntu-24.04)

# Update WSL
Open WSL
Update package index: sudo apt update
(Line above creates a list of available updates for installed packages)

# Check MPI installation
Install OpenMPI: sudo apt install openmpi-bin openmpi-common libopenmpi-dev
Check installation by asking for path to installation: which mpif90 (or which mpicc)
(If path is returned — something like /user/bin/mpicc — then MPI wrapper present)

# Check Python
Check python installed: python3 --version
If Python not installed: sudo apt install python3
If Python installed create symbolic link (shortcut — usr/bin/python): sudo ln -s /usr/bin/python3 /usr/bin/python
(this link needed during make process)

# Check Perl
Check perl installed: perl –version
If not installed: sudo apt install perl

# Check netCDF
Check netCDF installed: nc-config –all
If not installed: sudo apt update
Then: sudo apt install netcdf-bin libnetcdf-dev

# Check C compiler installed
Check C complier installed: gcc --version

# Check GNU Fortran compiler installed
Check C complier installed: gfortran --version
Install g++ package
sudo apt install g++

# Install netCDF-Fortran Library
sudo apt install libnetcdff-dev

# Install CMake package
sudo apt install cmake -y

# Compile using Cmake approach
Follow Cmake method with gfortran from manual (see online SCHISM manual)

Mount windows drive where SCHISM source code is installed: cd /mnt/c/SCHISM
(Above example is for a folder C:\SCHISM installed on local hard drive)

Create a ‘build’ folder: mkdir build
Navigate to build folder: cd build
Clean old cache: rm -rf *
Use default examples in cmake folder: cmake -C ../cmake/SCHISM.local.build -C ../cmake/SCHISM.local.ubuntu ../src/

Use make to generate executable: make -j8 pschism
(To run in better mode for debugging: make VERBOSE=1 pschism)
