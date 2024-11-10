Running SCHISM for the first time
# Check Windows in Command prompt
Press Win + R, type cmd, and press Enter, or search for "Command Prompt" in the Start menu
winver
This will open a window displaying your Windows version and build information
# Install Ubuntu linux on Windows
Click: https://www.youtube.com/watch?v=wjbbl0TTMeo&pp=ygUjaG93IHRvIGluc3RhbGwgdWJ1bnR1IG9uIHdpbmRvd3MgMTE%3D
# Ubuntu installed
To navigate to the C:\schism directory from within Ubuntu (assuming it’s mounted in the /mnt directory), you can use the following command
cd /mnt/c/schism
# Error: compiler mpif90
Install MPI. You can choose between openmpi or mpich, depending on your preference. To install openmpi, run: 
sudo apt update
sudo apt install openmpi-bin openmpi-common libopenmpi-dev
# No python
Install Python or link it correctly. First, check if Python 3 is installed
python3 --version
If Python 3 is installed, create a symlink so python points to python3
sudo ln -s /usr/bin/python3 /usr/bin/python
If Python is not installed, install it with:
sudo apt update
sudo apt install python3
# Error: Makefile:115: o/svn.BORA/autosrc.mk: No such file or directory
This error means autosrc.mk is missing.
Check if there’s an instruction to generate this file, or try running:
make clean
# Error after type: make pschism
# /bin/sh: 1: ../mk/sfmakedepend.pl: not found
Check if sfmakedepend.pl exists in your project. If not, it may have been missed during the project setup or download. Ensure that all files are fully downloaded or cloned from the repository.
If sfmakedepend.pl exists but is located elsewhere, you might need to modify the Makefile to point to its correct path.
# Perl Dependency
Install Perl if it’s missing:
sudo apt update
sudo apt install perl
# Reset building environment
make clean
make pschism
# To build the SCHISM code using CMake 
# Navigate to the cmake/ Directory
cd /path/to/schism/cmake
# Set Up SCHISM.local.build
TVD_LIM=1
BLD_STANDALONE=ON
NO_PARMETIS=ON    # (if bypassing ParMETIS)
OLDIO=ON          # (set to OFF if you want to use asynchronous I/O)
# Set Up SCHISM.local.cluster_name
cp -L SCHISM.local.whirlwind SCHISM.local.myown
# Run CMake
cd ..
mkdir build
cd build
Run cmake to configure the build:
cmake ..
This will read the CMake files and configure the build based on the settings you specified in the SCHISM.local.build and SCHISM.local.cluster_name files.
# Build the Code
Compile the project by running make within the build directory:
make -j$(nproc) pschism
