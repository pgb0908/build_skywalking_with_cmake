cmake_minimum_required(VERSION 3.10)
project(build_skywalk)

set(CMAKE_CXX_STANDARD 11)

add_executable(build_skywalk main.cpp )

# bazel build //source:cpp2sky_lib
include(ExternalProject)
set(source_dir ${CMAKE_SOURCE_DIR}/third/cpp2sky)

ExternalProject_Add(cpp2sky
        CONFIGURE_COMMAND cd ${source_dir}
        SOURCE_DIR ${source_dir}
        BUILD_COMMAND cd ${source_dir} && bazel build //source:cpp2sky_lib
        INSTALL_COMMAND echo "build libcpp2sky_lib.a"
        )

#[[add_library(cpp2sky_lib STATIC IMPORTED)
set_target_properties(cpp2sky_lib
        PROPERTIES
        IMPORTED_LOCATION  ${CMAKE_SOURCE_DIR}/third/cpp2sky/bazel-bin/source/libcpp2sky_lib.a)]]

add_dependencies(build_skywalk cpp2sky)

TARGET_LINK_LIBRARIES(build_skywalk ${CMAKE_SOURCE_DIR}/third/cpp2sky/bazel-bin/source/libcpp2sky_lib.a)
include_directories(${CMAKE_SOURCE_DIR}/third/cpp2sky/bazel-cpp2sky/)

