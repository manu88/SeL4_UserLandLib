cmake_minimum_required(VERSION 3.7.2)

project(libFoo C)

add_library(libFoo EXCLUDE_FROM_ALL src/foo.c)
target_include_directories(libFoo PUBLIC include)
target_link_libraries(libFoo muslc)
