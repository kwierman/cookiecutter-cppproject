cmake_minimum_required (VERSION 2.8)

project ({{ cookiecutter.project_name|lower }})

add_subdirectory(googletest)

file (GLOB srcs src/*.cc)
add_library ({{ cookiecutter.project_name|lower }} STATIC ${srcs})
target_include_directories ({{ cookiecutter.project_name|lower }} PRIVATE ${PROJECT_SOURCE_DIR}/include)

file (GLOB testSources test/*.cpp)
add_executable ({{ cookiecutter.project_name|lower }}test ${testSources})
target_link_libraries ({{ cookiecutter.project_name|lower }}test {{ cookiecutter.project_name|lower }} gmock gmock_main gtest pthread)
target_include_directories ({{ cookiecutter.project_name|lower }}test PRIVATE ${gtest_SOURCE_DIR}/include)
target_include_directories ({{ cookiecutter.project_name|lower }}test PRIVATE ${gmock_SOURCE_DIR}/include)
target_include_directories ({{ cookiecutter.project_name|lower }}test PRIVATE ${PROJECT_SOURCE_DIR}/include)

enable_testing ()

add_test (Unit {{ cookiecutter.project_name|lower }}test)

add_custom_target(unit COMMAND ${CMAKE_BINARY_DIR}/{{ cookiecutter.project_name|lower }}test --gtest_color=true)