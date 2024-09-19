cmake_minimum_required(VERSION 3.16)
project(mav_visualization)


set(CMAKE_MACOSX_RPATH 0)
add_definitions(-std=c++17)

find_package(ament_cmake REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(rclcpp REQUIRED)
find_package(visualization_msgs REQUIRED)

#############
# LIBRARIES #
#############

add_library(${PROJECT_NAME}
  src/marker_group.cpp
  src/leica_marker.cpp
  src/hexacopter_marker.cpp
)
ament_target_dependencies(${PROJECT_NAME} rclcpp Eigen3 visualization_msgs)
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>)


############
# BINARIES #
############
#add_executable(hexacopter_publisher
#    src/hexacopter_publisher.cpp
#)
#target_link_libraries(hexacopter_publisher ${PROJECT_NAME})
#
#
#add_executable(leica_publisher
#  src/leica_publisher.cpp
#)
#target_link_libraries(leica_publisher ${PROJECT_NAME})
ament_package()
