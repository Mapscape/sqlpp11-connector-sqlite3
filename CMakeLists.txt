# Copyright (c) 2013, Roland Bock
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without modification,
# are permitted provided that the following conditions are met:
#
#   Redistributions of source code must retain the above copyright notice, this
#   list of conditions and the following disclaimer.
#
#   Redistributions in binary form must reproduce the above copyright notice, this
#   list of conditions and the following disclaimer in the documentation and/or
#   other materials provided with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR
# ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
# ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

cmake_minimum_required(VERSION 2.6)

# Cygwin does not define WIN32 and warn if not use with this flag
set(CMAKE_LEGACY_CYGWIN_WIN32 0) # Remove when CMake >= 2.8.4 is required

project (sqlpp11-connector-sqlite3)
enable_testing()

message(STATUS "Using ${CMAKE_CXX_COMPILER} (compiler id: ${CMAKE_CXX_COMPILER_ID})")
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
	set(CMAKE_CXX_FLAGS "-Wall -std=c++11 -stdlib=libc++ ${CMAKE_CXX_FLAGS}")
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
	set(CMAKE_CXX_FLAGS "-Wall -std=c++11 ${CMAKE_CXX_FLAGS}")
endif ()

set(SQLPP11_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../sqlpp11/include" CACHE FILEPATH "Path to sqlpp11 includes")

if(NOT EXISTS ${SQLPP11_INCLUDE_DIR}/sqlpp11/exception.h)
  message(SEND_ERROR "Can't find header.h in ${SQLPP11_INCLUDE_DIR}")
endif()

message(SEND_MESSAGE "including sqlpp11 from ${SQLPP11_INCLUDE_DIR}")
include_directories("${SQLPP11_INCLUDE_DIR}")
set(include_dir "${PROJECT_SOURCE_DIR}/include")
file(GLOB_RECURSE sqlpp_headers ${include_dir}/*.h ${SQLPP11_INCLUDE_DIR}/*.h)
include_directories(${include_dir})



# Mapscape-specific alterations

# building tests requires some external dependencies that are not available within MSGeneralpp build
SET( SQLITE_RELEASE_DIR ${SQLITE_RELEASE_ROOT}/${CV_HOSTTYPE})
MESSAGE( SEND_MESSAGE "SQLITE release dir = ${SQLITE_RELEASE_DIR}")

INCLUDE_DIRECTORIES( "${SQLITE_RELEASE_DIR}/include")
LINK_DIRECTORIES(    "${SQLITE_RELEASE_DIR}/lib")

#add_subdirectory(tests)
# end Mapscape-specific alterations.

add_subdirectory(src)

install(DIRECTORY "${PROJECT_SOURCE_DIR}/include/sqlpp11" DESTINATION include)

