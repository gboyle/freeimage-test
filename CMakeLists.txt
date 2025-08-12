cmake_minimum_required(VERSION 3.23)

project(freeimage-test CXX)

find_package(freeimage CONFIG REQUIRED)

add_executable(freeimage-test main.cpp)

target_link_libraries(freeimage-test PRIVATE freeimage::FreeImage freeimage::FreeImagePlus)
