cmake_minimum_required(VERSION 2.8.11)
project(MOM6 C Fortran)

find_package(MPI REQUIRED)
add_definitions(${MPI_Fortran_COMPILE_FLAGS})
include_directories(${MPI_Fortran_INCLUDE_PATH})
link_directories(${MPI_Fortran_LIBRARIES})
set(CMAKE_C_COMPILER ${MPI_C_COMPILER})

if("${NETCDF_DIR}" STREQUAL "")
  execute_process(COMMAND nf-config --prefix OUTPUT_VARIABLE NETCDF_DIR
    OUTPUT_STRIP_TRAILING_WHITESPACE)
endif()
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}")
find_package(NetCDF REQUIRED COMPONENTS F90 C)
include_directories(${NETCDF_F90_INCLUDE_DIRS})

message("Using Fortran: ${CMAKE_Fortran_COMPILER_ID} and C: ${CMAKE_C_COMPILER_ID}")

if(CMAKE_Fortran_COMPILER_ID STREQUAL "Intel")
  set(CMAKE_Fortran_FLAGS
    "${CMAKE_Fortran_FLAGS} -fno-alias -auto -safe-cray-ptr -ftz -assume byterecl -i4 -r8 -nowarn -sox -traceback")
  set(CMAKE_Fortran_FLAGS_RELEASE
    "-O2 -debug minimal -fp-model source")
  set(CMAKE_Fortran_FLAGS_DEBUG
    "-g -O0 -check -check noarg_temp_created -check nopointer -warn -warn noerrors -fpe0 -ftrapuv")
elseif(CMAKE_Fortran_COMPILER_ID STREQUAL "GNU")
  set(CMAKE_Fortran_FLAGS
    "${CMAKE_Fortran_FLAGS} -fcray-pointer -fdefault-real-8 -fdefault-double-8 -Waliasing -ffree-line-length-none -fno-range-check")
  set(CMAKE_Fortran_FLAGS_RELEASE "-O2 -fbounds-check")
  set(CMAKE_Fortran_FLAGS_DEBUG "-O0 -g -W -fbounds-check -fbacktrace -ffpe-trap=invalid,zero,overflow")
endif()

if(CMAKE_C_COMPILER_ID STREQUAL "Intel")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -sox -traceback -O2 -debug minimal")
elseif(CMAKE_C_COMPILER_ID STREQUAL "GNU")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2")
endif()

message("Using Fortran flags: ${CMAKE_Fortran_FLAGS} ${CMAKE_Fortran_FLAGS_${CMAKE_BUILD_TYPE}}")
message("NetCDF libs: ${NETCDF_F90_LIBRARIES}")

add_definitions(-Duse_libMPI -Duse_netCDF -Duse_LARGEFILE -DSPMD -D__IFC)
add_subdirectory(FMS)
add_subdirectory(MOM6)
add_subdirectory(MOM6-SIS2-coupled)
