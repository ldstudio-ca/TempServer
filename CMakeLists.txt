cmake_minimum_required (VERSION 2.6)
project (TempServer)

SET(LIBUV_LINK_FLAG "-luv")
add_definitions(${LIBUV_LINK_FLAG})
SET( GCC_COVERAGE_COMPILE_FLAGS "-luv ")
SET( CMAKE_CXX_FLAGS "${GCC_COVERAGE_COMPILE_FLAGS}")
SET( CMAKE_EXE_LINKER_FLAGS "${GCC_COVERAGE_COMPILE_FLAGS}")
add_executable(TempServer webserver.cc http_parser.c)
