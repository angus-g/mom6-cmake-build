set(FMS_DIR "${SOURCE_ROOT_DIR}/FMS")

# FMS static library
file(GLOB_RECURSE FMS_SOURCE LIST_DIRECTORIES false
  ${FMS_DIR}/*.[fF]90 ${FMS_DIR}/*.c)
add_library(FMS ${FMS_SOURCE})
target_include_directories(FMS PRIVATE
  /usr/include ${FMS_DIR}/include ${FMS_DIR}/mosaic ${FMS_DIR}/drifters ${FMS_DIR}/fms ${FMS_DIR}/mpp/include)
