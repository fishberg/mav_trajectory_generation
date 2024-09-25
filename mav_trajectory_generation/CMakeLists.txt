cmake_minimum_required(VERSION 3.16)
project(mav_trajectory_generation)

set(CMAKE_MACOSX_RPATH 0)
add_definitions(-std=c++14)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
#find_package(nlopt REQUIRED)
find_package(NLopt REQUIRED)
#SET(NLOPT_LIBRARY "/usr/local/lib/libnlopt.so")
#SET(NLOPT_LIBRARIES "/usr/local/lib/libnlopt.so.0" "/usr/local/lib/libnlopt.so.0.10.0")
find_package(glog REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(mav_msgs REQUIRED)
find_package(PkgConfig)
find_package(ament_cmake_gtest REQUIRED)
pkg_check_modules(YamlCpp REQUIRED yaml-cpp>=0.5)

#message(MAV_MSGS_INCL="${mav_msgs_INCLUDE_DIRS}")
#foreach(dir ${mav_msgs_INCLUDE_DIRS})
#    message("Include directory: ${dir}")
#endforeach()
#include_directories(~/vnav/ws/lab4/src/mav_comm/mav_msgs/include)

#############
# LIBRARIES #
#############

add_library(${PROJECT_NAME}
  src/motion_defines.cpp
  src/polynomial.cpp
  src/segment.cpp
  src/timing.cpp
  src/trajectory.cpp
  src/trajectory_sampling.cpp
  src/vertex.cpp
  src/io.cpp
  src/rpoly/rpoly_ak1.cpp
)

#ament_target_dependencies(${PROJECT_NAME} ${YamlCpp_LIBRARIES})
ament_target_dependencies(${PROJECT_NAME} ${YamlCpp} rclcpp Eigen3 mav_msgs geometry_msgs)
target_link_libraries(${PROJECT_NAME} glog::glog)

target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>)
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>)

############
# BINARIES #
############
add_executable(polynomial_timing_evaluation src/polynomial_timing_evaluation.cpp)
target_link_libraries(polynomial_timing_evaluation ${PROJECT_NAME})

ament_target_dependencies(polynomial_timing_evaluation glog)

#########
# TESTS #
#########

#ament_add_gtest(test_polynomial test/test_polynomial.cpp)
#ament_target_dependencies(test_polynomial ${PROJECT_NAME})
#target_link_libraries(test_polynomial ${PROJECT_NAME})

#ament_add_gtest(test_polynomial_optimization test/test_polynomial_optimization.cpp)
#ament_target_dependencies(test_polynomial_optimization ${PROJECT_NAME})
#target_link_libraries(test_polynomial_optimization ${PROJECT_NAME})



##########
# EXPORT #
##########

#install(TARGETS ${PROJECT_NAME} test_polynomial test_polynomial_optimization DESTINATION lib/${PROJECT_NAME})
install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}Targets
        DESTINATION lib/${PROJECT_NAME})

install(
  DIRECTORY include/
  DESTINATION include/
)

ament_export_include_directories("include/${PROJECT_NAME}")

ament_export_targets(${PROJECT_NAME}Targets HAS_LIBRARY_TARGET)
# TODO: ament_export_dependencies(some_dependency) ???

ament_package()
