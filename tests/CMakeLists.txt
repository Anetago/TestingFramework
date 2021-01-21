cmake_minimum_required(VERSION 3.10)

project(anetago_test)

## variable 
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

## enable 'gtest_add_tests'
include(GoogleTest)

# # Download and unpack googletest at configure time
# execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
#     RESULT_VARIABLE result
#     WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/googletest-download )

# if(result)
#     message( "CMake step for googletest failed: ${result}")
# endif()

# execute_process(COMMAND ${CMAKE_COMMAND} --build .
#     RESULT_VARIABLE result
#     WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/googletest-download )
# if(result)
#     message("Build step for googletest failed: ${result}")
# endif()

# # Prevent overriding the parent project's compiler/linker
# # settings on Windows
# set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# # Add googletest directly to our build. This defines
# # the gtest and gtest_main targets.
# add_subdirectory(${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/googletest-src
#                  ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/googletest-build
#                  EXCLUDE_FROM_ALL)

# # The gtest/gtest_main targets carry header search path
# # dependencies automatically when using CMake 2.8.11 or
# # later. Otherwise we have to add them here ourselves.
# if (CMAKE_VERSION VERSION_LESS 2.8.11)
#   include_directories("${gtest_SOURCE_DIR}/include")
# endif()

## Source file
file(GLOB_RECURSE TEST_CASE_SOURCE
    ${CMAKE_CURRENT_SOURCE_DIR}/case/*.cxx
)

# Now simply link against gtest or gtest_main as needed. Eg
add_executable(${PROJECT_NAME} ${TEST_CASE_SOURCE})
target_link_libraries(${PROJECT_NAME} ${ANETAGO_GLOBAL_PROJECT_NAME} gtest gtest_main)

# add_test(NAME AnetagoFullTest
#      COMMAND $<TARGET_FILE:anetago_test>
#      WORKING_DIRECTORY ${${CMAKE_RUNTIME_OUTPUT_DIRECTORY}}/unittest)
gtest_add_tests(TARGET ${PROJECT_NAME})
