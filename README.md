# mom6-cmake-build

Use `cmake` to build the *ocean_only* and *MOM6-SIS2-coupled* configurations of
[MOM6](https://github.com/NOAA-GFDL/MOM6) with
[FMS](https://github.com/NOAA-GFDL/FMS).

## Instructions
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

4. Use `cmake` to build the `Makefile` including paths to dependent libraries.

   On most systems, the automatic configuration will figure out your MPI and NetCDF
   implementations:

       $ (cd build_debug && cmake -DCMAKE_BUILD_TYPE=DEBUG ../build_config)
       $ (cd build_release && cmake -DCMAKE_BUILD_TYPE=RELEASE ../build_config)
       
   On some systems (notably NCI's Raijin), the autoconfiguration may have trouble.
   These alternate commands are tested with `openmpi/1.8.8` and `netcdf/4.3.3.1`:
   
       $ (cd build_debug && FC=mpif90 CC=mpicc cmake \
            -DCMAKE_BUILD_TYPE=DEBUG \
            -DNETCDF_DIR="${NETCDF}" \
            -DNETCDF_F90_INCLUDE_DIR="${NETCDF}/include/Intel" \
            -DNETCDF_F90_LIBRARY="${NETCDF}/lib/Intel/libnetcdff.so" \
         ../build_config)
       $ (cd build_release && FC=mpif90 CC=mpicc cmake \
            -DCMAKE_BUILD_TYPE=RELEASE \
            -DNETCDF_DIR="${NETCDF}" \
            -DNETCDF_F90_INCLUDE_DIR="${NETCDF}/include/Intel" \
            -DNETCDF_F90_LIBRARY="${NETCDF}/lib/Intel/libnetcdff.so" \
         ../build_config)

5. Build the MOM6 and FMS components, where `N` is an integer character denoting
   the number of processes to use for the build process (or omit `N` to let Make
   automatically choose):

       $ cd build_debug && make -jN

   This will build the debug FMS and MOM6 modules and executables in their respective
   subfolders.  Change `debug` to `release` to build the optimised executable.
   
   To build a specific configuration of MOM6, ocean-only or coupled with SIS2, use the
   `MOM6` (i.e. `make -jN MOM6`) or `MOM6-SIS2-coupled` targets.
