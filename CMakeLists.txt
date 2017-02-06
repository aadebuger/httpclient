project( httpclient )
cmake_minimum_required( VERSION 2.8 )

set(CPP-NETLIB_BUILD_TESTS OFF)
# remove compiler checks if you are using VS2010, it supports
#   1. auto
#   2. lambda
#   3. const iterator
# and, sadly, it doesn't support `for(item, cotainer)'
include( CheckCXXCompilerFlag )
check_cxx_compiler_flag( "-std=c++11"   COMPILER_SUPPORTS_CXX11 )
check_cxx_compiler_flag( "-std=c++0x"   COMPILER_SUPPORTS_CXX0X )
if( COMPILER_SUPPORTS_CXX11 )
    if( CMAKE_COMPILER_IS_GNUCXX )
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11" )
    else()
        set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11" )
    endif()
elseif( COMPILER_SUPPORTS_CXX0X)
    set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x" )
else()
    # MSVC, On by default (if available)
endif()


find_package( Boost 1.54.0
  REQUIRED unit_test_framework system regex date_time thread filesystem
  program_options chrono atomic log)

add_subdirectory(deps/cxxopts)
add_subdirectory(deps/cpp-netlib)

if (Boost_FOUND)

    include_directories(
      ${Boost_INCLUDE_DIRS}
#      ${CPPNETLIB_INLUDE_DIRS}
      )
endif () 
add_executable( ${PROJECT_NAME} main.cpp )
include_directories(${cxxopts_SOURCE_DIR}/include )
#link_directories("/Users/aadebuger/Documents/cloud2017/httpclient/httpclientbuild/deps/cpp-netlib/libs/network/src")
#link_directories("/Users/aadebuger/Documents/cpp-netlib-0.12.0-final/build/libs/network/src/")

target_link_libraries ( ${PROJECT_NAME} cxxopts)
target_link_libraries( ${PROJECT_NAME} cppnetlib-client-connections)
target_link_libraries( ${PROJECT_NAME} network-uri)