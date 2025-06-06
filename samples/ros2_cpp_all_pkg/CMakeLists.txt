cmake_minimum_required(VERSION 3.8)
project(ros2_cpp_all_pkg)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(ros2_cpp_all_pkg_interfaces REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)
find_package(std_msgs REQUIRED)
find_package(std_srvs REQUIRED)

set(TARGET_NAME ros2_cpp_node_component)

add_library(${TARGET_NAME} SHARED src/ros2_cpp_node.cpp)

rclcpp_components_register_node(${TARGET_NAME}
  PLUGIN "ros2_cpp_all_pkg::Ros2CppNode"
  EXECUTABLE ros2_cpp_node
)

target_include_directories(${TARGET_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)
target_compile_features(${TARGET_NAME} PUBLIC c_std_99 cxx_std_17)

ament_target_dependencies(${TARGET_NAME}
  rclcpp
  rclcpp_action
  ros2_cpp_all_pkg_interfaces
  rclcpp_components
  rclcpp_lifecycle
  std_msgs
  std_srvs
)

install(TARGETS ${TARGET_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
  OPTIONAL
)

install(DIRECTORY config
  DESTINATION share/${PROJECT_NAME}
  OPTIONAL
)

ament_package()
