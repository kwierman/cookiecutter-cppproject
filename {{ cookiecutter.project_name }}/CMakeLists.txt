cmake_minimum_required (VERSION 2.8 FATAL_ERROR)

project ({{ cookiecutter.project_name|lower }})
set({{ cookiecutter.project_name|lower }}_MAJOR_VERSION 0)
set({{ cookiecutter.project_name|lower }}_MINOR_VERSION 1)
set({{ cookiecutter.project_name|lower }}_PATCH_VERSION 0)
set({{ cookiecutter.project_name|lower }}_VERSION
  ${{{ cookiecutter.project_name|lower }}_MAJOR_VERSION}.${{{ cookiecutter.project_name|lower }}_MINOR_VERSION}.${{{ cookiecutter.project_name|lower }}_PATCH_VERSION})

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(STATUS ${PROJECT_NAME} " requires an out-of-source build.")
    message(STATUS "Please remove these files from ${CMAKE_BINARY_DIR} first:")
    message(STATUS "CMakeCache.txt")
    message(STATUS "CMakeFiles")
    message(STATUS "Once these files are removed, create a separate directory")
    message(STATUS "and run CMake from there")
    message(FATAL_ERROR "in-source build detected")
endif()

find_package(Boost 1.63.0 REQUIRED COMPONENTS python thread multithreaded)
if(Boost_FOUND)
  include_directories(${Boost_INCLUDE_DIRS})
endif()

add_subdirectory(googletest)

file (GLOB srcs src/*.cc)
add_library ({{ cookiecutter.project_name|lower }} SHARED ${srcs})
target_include_directories ({{ cookiecutter.project_name|lower }} PRIVATE ${PROJECT_SOURCE_DIR}/include)
taret_link_libraries( {{ cookiecutter.project_name|lower }}  LINK_PUBLIC ${Boost_LIBRARIES} )

file (GLOB testSources test/*.cc)
add_executable ({{ cookiecutter.project_name|lower }}test ${testSources})
target_link_libraries ({{ cookiecutter.project_name|lower }}test {{ cookiecutter.project_name|lower }} gmock gmock_main gtest pthread)
target_include_directories ({{ cookiecutter.project_name|lower }}test PRIVATE ${gtest_SOURCE_DIR}/include)
target_include_directories ({{ cookiecutter.project_name|lower }}test PRIVATE ${gmock_SOURCE_DIR}/include)
target_include_directories ({{ cookiecutter.project_name|lower }}test PRIVATE ${PROJECT_SOURCE_DIR}/include)

enable_testing ()

add_test (Unit {{ cookiecutter.project_name|lower }}test)

add_custom_target(unit COMMAND ${CMAKE_BINARY_DIR}/{{ cookiecutter.project_name|lower }}test --gtest_color=true)

configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall 
  COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)

add_custom_target(setup
  COMMAND export PATH=$PATH:${CMAKE_INSTALL_PREFIX}/bin
  COMMAND export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:${CMAKE_INSTALL_PREFIX}/lib
  COMMAND export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:${CMAKE_INSTALL_PREFIX}/lib
  COMMAND export PYTHONPATH=$PYTHONPATH:${CMAKE_INSTALL_PREFIX}/lib
  )
