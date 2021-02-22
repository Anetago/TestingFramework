cmake_minimum_required(VERSION 3.18)

project(AnetagoTesting CXX)
set(ANETAGO_GLOBAL_PROJECT_NAME ${PROJECT_NAME})

## include directory
include_directories(include)

set(PROJECT_PUBLIC_HEADERS
  include/anetago/testing.hh
)

## #include files
file(GLOB_RECURSE PROJECT_HEADERS
    ${PROJECT_SOURCE_DIR}/include/anetago/*.hh
)
 
## Source file
file(GLOB_RECURSE PROJECT_SOURCE
    ${PROJECT_SOURCE_DIR}/src/anetago/*.cxx
)

## Add test folder
configure_file(./cmake/google-test.cmake googletest-download/CMakeLists.txt)

# Download and unpack googletest at configure time
execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
    RESULT_VARIABLE result
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/googletest-download )

if(result)
    message( "CMake step for googletest failed: ${result}")
endif()

execute_process(COMMAND ${CMAKE_COMMAND} --build .
    RESULT_VARIABLE result
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/googletest-download )
if(result)
    message("Build step for googletest failed: ${result}")
endif()

# Prevent overriding the parent project's compiler/linker
# settings on Windows
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# Add googletest directly to our build. This defines
# the gtest and gtest_main targets.
add_subdirectory(${CMAKE_BINARY_DIR}/googletest-src
                 ${CMAKE_BINARY_DIR}/googletest-build
                 EXCLUDE_FROM_ALL)

# The gtest/gtest_main targets carry header search path
# dependencies automatically when using CMake 2.8.11 or
# later. Otherwise we have to add them here ourselves.
if (CMAKE_VERSION VERSION_LESS 2.8.11)
  include_directories("${gtest_SOURCE_DIR}/include")
endif()

enable_testing()
add_subdirectory(tests)

# Now simply link against gtest or gtest_main as needed. Eg
# add_executable(example src/testcase.cxx)
add_library(${ANETAGO_GLOBAL_PROJECT_NAME} STATIC ${PROJECT_SOURCE} ${PROJECT_HEADERS} ${PROJECT_PUBLIC_HEADERS})

