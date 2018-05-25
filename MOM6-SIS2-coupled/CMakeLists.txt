set(MOM6_DIR "${SOURCE_ROOT_DIR}/MOM6")
set(SIS2_DIR "${SOURCE_ROOT_DIR}/SIS2")

# common files for both builds
file(GLOB_RECURSE MOM6_SOURCE LIST_DIRECTORIES false FOLLOW_SYMLINKS
  ${MOM6_DIR}/src/*.[fF]90 ${MOM6_DIR}/src/*.c)

# MOM6-SIS2 coupled mode
add_definitions(-Duse_libMPI -Duse_netCDF -DSPMD -DUSE_LOG_DIAG_FIELD_INFO
    -Duse_AM3_physics -D_USE_LEGACY_LAND_)
file(GLOB_RECURSE MOM6_SIS2_COUPLED_SOURCE LIST_DIRECTORIES false
  ${MOM6_DIR}/config_src/coupled_driver/*.[fF]90
  ${MOM6_DIR}/config_src/dynamic/*.[fF]90
  ${SOURCE_ROOT_DIR}/atmos_null/*.[fF]90
  ${SOURCE_ROOT_DIR}/coupler/*.[fF]90
  ${SOURCE_ROOT_DIR}/land_null/*.[fF]90
  ${SOURCE_ROOT_DIR}/ice_ocean_extras/*.[fF]90
  ${SOURCE_ROOT_DIR}/icebergs/*.[fF]90
  ${SOURCE_ROOT_DIR}/SIS2/*.[fF]90
  ${SOURCE_ROOT_DIR}/FMS/coupler/*.[fF]90)
add_executable(MOM6-SIS2-coupled ${MOM6_SOURCE} ${MOM6_SIS2_COUPLED_SOURCE})
target_include_directories(MOM6-SIS2-coupled PRIVATE
    $<TARGET_FILE_DIR:FMS> ${MOM6_DIR}/config_src/dynamic
    ${SIS2_DIR}/config_src/dynamic ${SIS2_DIR}/src
    ${MOM6_DIR}/src/framework ${SOURCE_ROOT_DIR}/SIS2
    ${SOURCE_ROOT_DIR}/FMS/include)
target_link_libraries(MOM6-SIS2-coupled FMS z ${NETCDF_LIBRARIES}
  ${MPI_Fortran_LIBRARIES})
