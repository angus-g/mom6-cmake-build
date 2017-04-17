# mom6-cmake-build

Use `cmake` to build [MOM6](https://github.com/NOAA-GFDL/MOM6) and 
[FMS](https://github.com/NOAA-GFDL/FMS).

## Instructions
1. Clone this repository to the root folder of 
   [MOM6-examples](https://github.com/NOAA-GFDL/MOM6-examples):

   `$ git clone git@github.com:angus-g/mom6-cmake-build`

2. The preprocessor conditionals in `fft.F90` of FMS do not play well with 
   CMake.  Open `src/FMS/fft/fft.F90` and copy the contents of line *48* (`use 
   fft99_mod, only: fft991, set99` to line *46*.  Save and exit.

3. Use `cmake` to build the `Makefile` including paths to dependent libraries:

   `$ cd mom6-cmake-build && cmake .`

4. Build the MOM6 and FMS components, where `N` is an integer character denoting 
   the number of processes to use for the build process:

   `$ make -jN`

   This will build the FMS and MOM6 modules and executables in their respective 
   subfolders.
