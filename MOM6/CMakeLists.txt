set(MOM6_DIR "${CMAKE_SOURCE_DIR}/../src/MOM6")

# common files for both builds
file(GLOB_RECURSE MOM6_SOURCE LIST_DIRECTORIES false FOLLOW_SYMLINKS
  ${MOM6_DIR}/src/*.[fF]90 ${MOM6_DIR}/src/*.c)

# MOM6 ocean only
file(GLOB_RECURSE MOM6_SOLO_SOURCE LIST_DIRECTORIES false
  ${MOM6_DIR}/config_src/solo_driver/*.[fF]90)
add_executable(MOM6 ${MOM6_SOURCE} ${MOM6_SOLO_SOURCE})
target_include_directories(MOM6 PRIVATE
  $<TARGET_FILE_DIR:FMS> ${MOM6_DIR}/config_src/dynamic ${MOM6_DIR}/src/framework)
target_link_libraries(MOM6 FMS z ${NETCDF_F90_LIBRARIES} ${MPI_Fortran_LIBRARIES})
