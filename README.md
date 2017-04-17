# mom6-cmake-build

Use `cmake` to build the *ocean_only* and *MOM6-SIS2-coupled* configurations of
[MOM6](https://github.com/NOAA-GFDL/MOM6) with
[FMS](https://github.com/NOAA-GFDL/FMS).

## Instructions
0. **Optional**: Follow [these 
   steps](https://gist.github.com/anders-dc/5b49f181b003c1995a28802f5d9fe329) to 
   set up the build environment.

1. Clone this repository to the root folder of
   [MOM6-examples](https://github.com/NOAA-GFDL/MOM6-examples):

       $ git clone https://github.com/angus-g/mom6-cmake-build build_config

2. **Temporary step:**
   The preprocessor conditionals in `fft.F90` of FMS do not play well with
   CMake.  Open `src/FMS/fft/fft.F90` and copy the contents of line *48* (`use
   fft99_mod, only: fft991, set99` to line *46* (outside the preprocessor
   conditional block).  Save and exit.

3. Make directories for debug and release builds:

       $ mkdir build_debug build_release

4. Use `cmake` to build the `Makefile` including paths to dependent libraries:

       $ (cd build_debug && cmake -DCMAKE_BUILD_TYPE=DEBUG ../build_config)
       $ (cd build_release && cmake -DCMAKE_BUILD_TYPE=RELEASE ../build_config)

5. Build the MOM6 and FMS components, where `N` is an integer character denoting
   the number of processes to use for the build process (or omit `N` to let Make
   automatically choose):

       $ cd build_debug && make -jN

   This will build the debug FMS and MOM6 modules and executables in their respective
   subfolders.  Change `debug` to `release` to build the optimised executable.
