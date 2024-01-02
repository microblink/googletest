# Note: CMake support is community-based. The maintainers do not use CMake
# internally.

cmake_minimum_required(VERSION 2.8.12)

if (POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

project(googletest-distribution)
set(GOOGLETEST_VERSION 1.11.0)

if (CMAKE_VERSION VERSION_GREATER "3.0.2")
  if(NOT CYGWIN AND NOT MSYS AND NOT ${CMAKE_SYSTEM_NAME} STREQUAL QNX)
    set(CMAKE_CXX_EXTENSIONS OFF)
  endif()
endif()

enable_testing()

include(CMakeDependentOption)
include(GNUInstallDirs)

#Note that googlemock target already builds googletest
option(BUILD_GMOCK "Builds the googlemock subproject" ON)
option(INSTALL_GTEST "Enable installation of googletest. (Projects embedding googletest may want to turn this OFF.)" ON)

if ( ANDROID )
    option( GTEST_REDIRECT_OUTPUT_TO_ANDROID_LOG "Redirect GTest output to Android Log" ON )

    if( GTEST_REDIRECT_OUTPUT_TO_ANDROID_LOG )
        add_definitions( -DGTEST_REDIRECT_TO_ANDROID_LOG )
    endif()
endif()

if ( CLANG )
    # char_traits< signed char > is deprecated, but we still need it until we switch MB::Utf8Char to char8_t instead of signed char
    add_compile_options( -Wno-deprecated-declarations )
endif()

if(BUILD_GMOCK)
  add_subdirectory( googlemock )
else()
  add_subdirectory( googletest )
endif()
