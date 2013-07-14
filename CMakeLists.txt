# Copyright (C) 2013
# Martin Lambers <marlam@marlam.de>
#
# Copying and distribution of this file, with or without modification, are
# permitted in any medium without royalty provided the copyright notice and this
# notice are preserved. This file is offered as-is, without any warranty.

cmake_minimum_required(VERSION 2.8)

# Configuration
set(PACKAGE_NAME "eqvnc")
set(PACKAGE_VERSION "1.0")
project(${PACKAGE_NAME} C CXX)
if(CMAKE_COMPILER_IS_GNUCXX)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")
endif()

# Find libraries
find_package(OpenGL REQUIRED)
find_package(PkgConfig)
pkg_check_modules(LIBVNCCLIENT REQUIRED libvncclient)
pkg_check_modules(EQUALIZER REQUIRED Equalizer)

# Main target
add_executable(eqvnc eqvnc.cpp)
include_directories(${CMAKE_SOURCE_DIR} ${LIBVNCCLIENT_INCLUDE_DIRS} ${EQUALIZER_INCLUDE_DIRS})
target_link_libraries(eqvnc ${OPENGL_gl_LIBRARY} ${LIBVNCCLIENT_LIBRARIES} ${EQUALIZER_LIBRARIES})
install(TARGETS eqvnc RUNTIME DESTINATION bin)

# Extra target: 'make dist'
set(ARCHIVE_NAME ${PACKAGE_NAME}-${PACKAGE_VERSION})
add_custom_target(dist
  COMMAND git archive --prefix=${ARCHIVE_NAME}/ HEAD
    | xz > ${CMAKE_BINARY_DIR}/${ARCHIVE_NAME}.tar.xz
  WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})