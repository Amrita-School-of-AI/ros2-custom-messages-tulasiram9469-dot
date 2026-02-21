cmake_minimum_required(VERSION 3.8)
project(ros2_custom_msgs)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

# Find required packages
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rosidl_default_generators REQUIRED)

# Generate custom message
rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/RobotStatus.msg"
)

# Create executable
add_executable(status_publisher
  src/status_publisher.cpp
)

ament_target_dependencies(status_publisher
  rclcpp
)

# 🔹 NEW Jazzy-compatible way to link message typesupport
rosidl_get_typesupport_target(cpp_typesupport_target
  ${PROJECT_NAME}
  "rosidl_typesupport_cpp"
)

target_link_libraries(status_publisher
  ${cpp_typesupport_target}
)

# Install executable
install(TARGETS
  status_publisher
  DESTINATION lib/${PROJECT_NAME}
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()
