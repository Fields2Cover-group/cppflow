cmake_minimum_required(VERSION 3.8 FATAL_ERROR)

project(cppflow LANGUAGES CXX)
find_library(TENSORFLOW_LIB tensorflow HINT $ENV{HOME}/libtensorflow2/lib)
set(CMAKE_CXX_STANDARD 17)

add_library(cppflow INTERFACE)


set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
set(INSTALL_LIB_DIR lib CACHE PATH "Installation directory for libraries")
set(INSTALL_BIN_DIR bin CACHE PATH "Installation directory for executables")
set(INSTALL_INCLUDE_DIR include CACHE PATH
  "Installation directory for header files")


foreach(p LIB BIN INCLUDE CMAKE)
  set(var INSTALL_${p}_DIR)
  if(NOT IS_ABSOLUTE "${${var}}")
    set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  endif()
endforeach()


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

install(TARGETS cppflow
  EXPORT cppflow-targets
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/cppflow"
)

install(EXPORT cppflow-targets
  FILE cppflowConfig.cmake
  DESTINATION ${INSTALL_CMAKE_DIR}/cmake/cppflow
)

export(TARGETS cppflow
  FILE cppflowConfig.cmake)


set(CMAKE_EXPORT_PACKAGE_REGISTRY ON)
export(PACKAGE cppflow)
