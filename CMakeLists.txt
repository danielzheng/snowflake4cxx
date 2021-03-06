cmake_minimum_required(VERSION 2.6)

project(snowflake4cxx C CXX)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Debug")
endif()

set(CXX_FLAGS
 -g
 # -DVALGRIND
 # -DCHECK_PTHREAD_RETURN_VALUE
 -D_FILE_OFFSET_BITS=64
 -Wall
 -Wextra
 # -Werror
 -Wconversion
 -Wno-unused-parameter
 -Wno-sign-conversion
 -Wno-old-style-cast
 -Woverloaded-virtual
 -Wpointer-arith
 -Wshadow
 -Wwrite-strings
 # -Wno-deprecated
 -DFOLLY_HAVE_CLOCK_GETTIME=1
 
 -march=native
 # -MMD
 -std=gnu++1y

 # -Wold-style-cast
 # -Wunused-parameter
 )

if(CMAKE_BUILD_BITS EQUAL 32)
  list(APPEND CXX_FLAGS "-m32")
endif()

string(REPLACE ";" " " CMAKE_CXX_FLAGS "${CXX_FLAGS}")

if(APPLE)
  set(CMAKE_CXX_COMPILER "g++")
#  set(CMAKE_CXX_COMPILER "clang++")
elseif(UNIX)
  set(CMAKE_CXX_COMPILER "g++")
endif()

set(CMAKE_CXX_FLAGS_DEBUG "-O0")
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG")

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

include_directories(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR}/nebula)

#string(TOUPPER ${CMAKE_BUILD_TYPE} BUILD_TYPE)
message(STATUS "CXX_FLAGS = " ${CMAKE_CXX_FLAGS} " " ${CMAKE_CXX_FLAGS_${BUILD_TYPE}})

if(APPLE)
include_directories(/usr/local/Cellar/openssl/1.0.2d_1/include /opt/local/include /usr/local/include)
link_directories(/usr/local/Cellar/openssl/1.0.2d_1/lib /opt/local/lib /usr/local/lib /usr/local/mysql/lib ${PROJECT_BINARY_DIR}/lib)
elseif(UNIX)
include_directories(/opt/im-cloud-server-thirdparty/linux/include)
link_directories(/opt/im-cloud-server-thirdparty/linux/lib /usr/local/lib /usr/local/mysql/lib ${PROJECT_BINARY_DIR}/lib)
set(CMAKE_EXE_LINKER_FLAGS "-lpthread -lrt -ldl")
endif()

add_subdirectory(snowflake4cxx)

