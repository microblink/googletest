cmake_minimum_required(VERSION 2.6.2)

project( googletest-distribution )

enable_testing()

option(BUILD_GTEST "Builds the googletest subproject" OFF)

#Note that googlemock target already builds googletest
option(BUILD_GMOCK "Builds the googlemock subproject" ON)

if ( ANDROID )
    option(GTEST_REDIRECT_OUTPUT_TO_ANDROID_LOG "Redirect GTest output to Android Log" OFF)

    if( GTEST_REDIRECT_OUTPUT_TO_ANDROID_LOG )
        add_definitions( -DGTEST_REDIRECT_TO_ANDROID_LOG )
    endif()
endif()

if(BUILD_GMOCK)
  add_subdirectory( googlemock )
elseif(BUILD_GTEST)
  add_subdirectory( googletest )
endif()
