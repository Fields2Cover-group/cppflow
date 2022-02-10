cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(cppflow LANGUAGES CXX)
find_library(TENSORFLOW_LIB tensorflow HINT $ENV{HOME}/libtensorflow2/lib)
set(CMAKE_CXX_STANDARD 17)

add_library(cppflow INTERFACE)

target_compile_features(cppflow INTERFACE cxx_std_17)

target_include_directories(cppflow INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

target_link_libraries(cppflow INTERFACE
  ${TENSORFLOW_LIB}
)

set_target_properties(cppflow PROPERTIES
   POSITION_INDEPENDENT_CODE ON
)

install(TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}-targets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}"
)

export(PACKAGE cppflow)
