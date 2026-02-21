cmake_minimum_required(VERSION 3.8)
project(ros2_custom_msgs)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
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

# Ensure message headers are generated before compiling executable
add_dependencies(status_publisher
  ${PROJECT_NAME}_cpp
)

# Link dependencies
ament_target_dependencies(status_publisher
  rclcpp
)

# Link message type support
rosidl_target_interfaces(status_publisher
  ${PROJECT_NAME} "rosidl_typesupport_cpp"
)

# Install executable
install(TARGETS
  status_publisher
  DESTINATION lib/${PROJECT_NAME}
)

# Export dependencies
ament_export_dependencies(rosidl_default_runtime)

ament_package()
