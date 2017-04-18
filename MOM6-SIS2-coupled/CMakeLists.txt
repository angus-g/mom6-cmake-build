set(MOM6_DIR "${CMAKE_SOURCE_DIR}/../src/MOM6")

# common files for both builds
file(GLOB_RECURSE MOM6_SOURCE LIST_DIRECTORIES false FOLLOW_SYMLINKS
  ${MOM6_DIR}/src/*.[fF]90 ${MOM6_DIR}/src/*.c)

# MOM6-SIS2 coupled mode
add_definitions(-Duse_libMPI -Duse_netCDF -DSPMD -DUSE_LOG_DIAG_FIELD_INFO -Duse_AM3_physics)
file(GLOB_RECURSE MOM6_SIS2_COUPLED_SOURCE LIST_DIRECTORIES false
  ${MOM6_DIR}/config_src/coupled_driver/*.[fF]90
  ${MOM6_DIR}/config_src/dynamic/*.[fF]90
  ${MOM6_DIR}/../atmos_null/*.[fF]90
  ${MOM6_DIR}/../coupler/*.[fF]90
  ${MOM6_DIR}/../land_null/*.[fF]90
  ${MOM6_DIR}/../ice_ocean_extras/*.[fF]90
  ${MOM6_DIR}/../icebergs/*.[fF]90
  ${MOM6_DIR}/../SIS2/*.[fF]90
  ${MOM6_DIR}/../FMS/coupler/*.[fF]90)
add_executable(MOM6-SIS2-coupled ${MOM6_SOURCE} ${MOM6_SIS2_COUPLED_SOURCE})
target_include_directories(MOM6-SIS2-coupled PRIVATE
  $<TARGET_FILE_DIR:FMS> ${MOM6_DIR}/config_src/dynamic 
  ${MOM6_DIR}/src/framework ${MOM6_DIR}/../SIS2 ${MOM6_DIR}/../FMS/include)
target_link_libraries(MOM6-SIS2-coupled FMS z ${NETCDF_F90_LIBRARIES} 
  ${NETCDF_C_LIBRARIES} ${MPI_Fortran_LIBRARIES})
