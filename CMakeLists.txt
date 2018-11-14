cmake_minimum_required(VERSION 2.8.3)
project(wm_odas_ros_wrapper)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)


# Matrix creator HAL deps
find_library(MATRIX_HAL_LIBRARIES NAMES wm_odas_ros_wrapper)
find_library(FFTW_LIBRARIES NAMES fftw3f)
find_library(WIRINGPI_LIBRARIES NAMES wiringPi)

find_path (JSON_C_INCLUDE_DIR json.h PATH_SUFFIXES json-c json)
find_library (JSON_C_LIBRARIES NAMES json-c libjson-c libjson-c-dev)
include_directories (${JSON_C_INCLUDE_DIR})


set(MATRIX_CREATOR_LIBRARIES ${MATRIX_HAL_LIBRARIES} ${FFTW_LIBRARIES} ${WIRINGPI_LIBRARIES})

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  roscpp
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

catkin_package()

add_executable(test_odas src/test_odas.cpp)

# Specify libraries to link a library or executable target against
target_link_libraries(test_odas ${catkin_LIBRARIES} ${MATRIX_CREATOR_LIBRARIES} ${JSON_C_LIBRARIES})
