cmake_minimum_required(VERSION 3.5)

project(clion_include_bug_example)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED ON)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)

include_directories(include)

function(add_demo_dependencies library)
  target_compile_definitions(${library}
    PRIVATE "DEMO_NODES_CPP_BUILDING_DLL")
  ament_target_dependencies(${library}
    "rclcpp"
    "rclcpp_components"
    "std_msgs")
endfunction()

add_library(timers_library SHARED
  src/timers/one_off_timer.cpp
  src/timers/reuse_timer.cpp)
add_demo_dependencies(timers_library)

rclcpp_components_register_node(timers_library
  PLUGIN "demo_nodes_cpp::OneOffTimerNode"
  EXECUTABLE one_off_timer)
rclcpp_components_register_node(timers_library
  PLUGIN "demo_nodes_cpp::ReuseTimerNode"
  EXECUTABLE reuse_timer)

install(TARGETS
  timers_library
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)

ament_package()
