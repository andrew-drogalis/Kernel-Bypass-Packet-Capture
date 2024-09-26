cmake_minimum_required(VERSION 3.20)
project(DPDKLinuxKernelBypassMulticast)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
include(GNUInstallDirs)

include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${GTEST_INCLUDE_DIRS})

find_package(GTest)

enable_testing()

add_executable(${PROJECT_NAME} src/multicast-main.cpp)

target_include_directories(${PROJECT_NAME} PRIVATE /src)

# target_link_libraries(${PROJECT_NAME} LINK_PUBLIC GTest::GTest GTest::Main)

include(cmake/CompilerWarnings.cmake)
include(cmake/Sanitizers.cmake)

myproject_set_project_warnings(${PROJECT_NAME} TRUE "X" "" "" "X")

add_compile_options(-pipe -fPIC)
myproject_enable_sanitizers(${PROJECT_NAME} TRUE TRUE TRUE FALSE TRUE)

# we cannot analyse results without gcov
find_program(GCOV_PATH gcov)
if(NOT GCOV_PATH)
  message(FATAL_ERROR "Code coverage analysis requires gcov!")
endif()

# include(GoogleTest)
# gtest_discover_tests(${PROJECT_NAME})
