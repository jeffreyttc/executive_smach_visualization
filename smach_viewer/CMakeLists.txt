cmake_minimum_required(VERSION 3.5)
project(smach_viewer)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

message(STATUS "Installing system python-gi python-gi-cairo libgtk-3-dev")
execute_process(COMMAND apt-get install -y python-gi python-gi-cairo python-wxversion libgtk-3-dev)

message(STATUS "Installing python3 wxpython graphviz")
execute_process(COMMAND pip3 install wxpython graphviz)

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclpy REQUIRED)
find_package(smach_msgs REQUIRED)

ament_python_install_package(${PROJECT_NAME})

install(PROGRAMS
  ${PROJECT_NAME}/smach_viewer_gui.py
  DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  list(APPEND AMENT_LINT_AUTO_EXCLUDE ament_cmake_copyright)
  ament_lint_auto_find_test_dependencies()

  # find_package(ament_cmake_pytest REQUIRED)
  # ament_add_pytest_test(${PROJECT_NAME} test
  #   APPEND_ENV PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR})

  find_package(ament_cmake_flake8 REQUIRED)
endif()

ament_export_dependencies(ament_cmake)
ament_export_dependencies(ament_cmake_python)
ament_package()
