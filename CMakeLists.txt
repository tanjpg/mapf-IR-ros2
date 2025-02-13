cmake_minimum_required(VERSION 3.8)
project(mapf_ros2)
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
find_package(ament_cmake REQUIRED)

add_subdirectory(./mapf-IR/mapf)
add_custom_target(clang-format
  COMMAND clang-format -i
  ./mapf-IR/mapf/include/*.hpp
  ./mapf-IR/mapf/src/*.cpp)

install(
  DIRECTORY ./mapf-IR/third_party/grid-pathfinding/graph/include/  
  DESTINATION include
)

install(
  DIRECTORY ./mapf-IR/mapf/include/  
  DESTINATION include/mapf
)

install(
  TARGETS lib-mapf lib-graph
  EXPORT export_lib-mapf
  RUNTIME DESTINATION bin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  INCLUDES DESTINATION include
)

ament_export_targets(export_lib-mapf HAS_LIBRARY_TARGET)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
