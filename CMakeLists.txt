# Sets the minimum version of CMake required to build the native
# library. You should either keep the default value or only pass a
# value of 3.4.0 or lower.
# by huzongyao

cmake_minimum_required(VERSION 3.0)

option (BUILD_SHARED_LIB "compatible format" ON )

include("../cmake/source_group_by_dir.cmake")


# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds it for you.
# Gradle automatically packages shared libraries with your APK.
file(GLOB_RECURSE LFS_SRCS 
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
)
# message(stats "${LFS_SRCS}")

message("CMAKE_SOURCE_DIR=${CMAKE_SOURCE_DIR}")

include_directories(
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/
    ${CMAKE_CURRENT_SOURCE_DIR}/../lua
)


link_directories(
    ${CMAKE_BINARY_DIR}/bin
    ${CMAKE_BINARY_DIR}/lib
)

if (BUILD_SHARED_LIB)
    add_library(lfs SHARED ${LFS_SRCS})
    target_link_libraries(lfs liblua)
else()
    add_library(lfs STATIC ${LFS_SRCS})
endif(BUILD_SHARED_LIB)
source_group_by_dir(${CMAKE_CURRENT_SOURCE_DIR} LFS_SRCS)
install_library(lfs)
