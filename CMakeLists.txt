cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(cppflow LANGUAGES CXX)
find_library(TENSORFLOW_LIB tensorflow HINT $ENV{HOME}/libtensorflow2/lib)
set(CMAKE_CXX_STANDARD 17)

add_library(cppflow INTERFACE)

target_include_directories(cppflow INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)
target_compile_features(cppflow INTERFACE cxx_std_17)
target_link_libraries(cppflow INTERFACE
  ${TENSORFLOW_LIB}
)

install(
  TARGETS cppflow
  EXPORT install_targets
)

#set(CMAKE_EXPORT_PACKAGE_REGISTRY ON)
#install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
export(PACKAGE cppflow)
