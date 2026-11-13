cmake_minimum_required(VERSION 3.8)
project(rover_description)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  set(ament_cmake_copyright_FOUND TRUE)
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

install(DIRECTORY
  launch
  urdf
  meshes
  config
  world
  DESTINATION share/${PROJECT_NAME}
)

find_program(XACRO_EXECUTABLE xacro)

add_custom_command(
  OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/urdf/simp_rover.urdf
  COMMAND ${XACRO_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/urdf/simp_rover.xacro
          config_path:=${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME}/config/rover_controller.yaml
          > ${CMAKE_CURRENT_SOURCE_DIR}/urdf/simp_rover.urdf
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/urdf/simp_rover.xacro
  COMMENT "Generating static simp_rover.urdf from simp_rover.xacro"
)

add_custom_target(generate_simp_rover_urdf ALL
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/urdf/simp_rover.urdf
)

ament_package()
