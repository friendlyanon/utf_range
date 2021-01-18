# Copyright (c) Darrell Wright
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE or copy at http://www.boost.org/LICENSE_1_0.txt)
#
# Official repository: https://github.com/beached/utf_range
#

cmake_minimum_required(VERSION 3.14) 

project(
    utf_range
    VERSION 1.1.1
    DESCRIPTION "UTF8 Iterator/Range for traversing utf8 text"
    HOMEPAGE_URL "https://github.com/beached/utf_range"
    LANGUAGES CXX
)

set(daw_utf_range_warning_guard SYSTEM)
if(daw_utf_range_INCLUDE_WITHOUT_SYSTEM)
  set(daw_utf_range_warning_guard "")
endif()

add_library(utf_range INTERFACE) 
add_library(daw::utf_range ALIAS utf_range)

target_compile_features(utf_range INTERFACE cxx_std_17)
target_include_directories(
    utf_range
    ${daw_utf_range_warning_guard}
    INTERFACE
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>"
)

find_package(header_libraries CONFIG REQUIRED)
target_link_libraries(utf_range INTERFACE daw::header_libraries)

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

set(utf_range_directory "utf_range-${PROJECT_VERSION}")
set(
    utf_range_include_directory
    "${CMAKE_INSTALL_INCLUDEDIR}/${utf_range_directory}"
)

install(
    DIRECTORY "${PROJECT_SOURCE_DIR}/include/"
    DESTINATION "${utf_range_include_directory}"
    COMPONENT utf_range_Development
)

install(
  TARGETS utf_range
	EXPORT utf_rangeTargets
  INCLUDES DESTINATION "${utf_range_include_directory}"
)

write_basic_package_version_file(
    utf_rangeConfigVersion.cmake
    COMPATIBILITY SameMajorVersion
    ARCH_INDEPENDENT
)

set(
    utf_range_install_cmakedir
    "${CMAKE_INSTALL_LIBDIR}/cmake/${utf_range_directory}"
)

install(
    FILES "${PROJECT_BINARY_DIR}/utf_rangeConfigVersion.cmake"
    DESTINATION "${utf_range_install_cmakedir}"
    COMPONENT utf_range_Development
)

install(
    EXPORT utf_rangeTargets
    FILE utf_rangeConfig.cmake
    NAMESPACE daw::
    DESTINATION "${utf_range_install_cmakedir}"
    COMPONENT utf_range_Development
)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
  include(CPack)
endif()
