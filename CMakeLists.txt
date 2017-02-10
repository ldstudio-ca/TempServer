cmake_minimum_required(VERSION 3.6)
project(TempServer)

set(CMAKE_CXX_STANDARD 11)

set(LIBUVDIR /Users/larry/Projects/libuv)
include_directories(${LIBUVDIR}/include ${LIBUVDIR}/src /usr/local/lib/)

set(SOURCES
  ${LIBUVDIR}/src/fs-poll.c
  ${LIBUVDIR}/src/inet.c
  ${LIBUVDIR}/src/threadpool.c
  ${LIBUVDIR}/src/uv-common.c
  ${LIBUVDIR}/src/version.c)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  add_definitions(-D_GNU_SOURCE)
  set(SOURCES ${SOURCES}
    ${LIBUVDIR}/src/unix/linux-syscalls.c
    ${LIBUVDIR}/src/unix/linux-core.c
    ${LIBUVDIR}/src/unix/linux-inotify.c)
elseif(${CMAKE_SYSTEM_NAME} STREQUAL "Darwin")
  add_definitions(-D_DARWIN_USE_64_BIT_INODE=1 -D_DARWIN_UNLIMITED_SELECT=1)
  set(SOURCES ${SOURCES}
    ${LIBUVDIR}/src/unix/darwin.c
    ${LIBUVDIR}/src/unix/darwin-proctitle.c
    ${LIBUVDIR}/src/unix/fsevents.c
    ${LIBUVDIR}/src/unix/kqueue.c
    ${LIBUVDIR}/src/unix/pthread-barrier.c
    ${LIBUVDIR}/src/unix/proctitle.c)
endif()

include_directories(${LIBUVDIR}/src/unix)
set(SOURCES ${SOURCES}
  ${LIBUVDIR}/src/unix/async.c
  ${LIBUVDIR}/src/unix/core.c
  ${LIBUVDIR}/src/unix/dl.c
  ${LIBUVDIR}/src/unix/fs.c
  ${LIBUVDIR}/src/unix/getaddrinfo.c
  ${LIBUVDIR}/src/unix/getnameinfo.c
  ${LIBUVDIR}/src/unix/loop-watcher.c
  ${LIBUVDIR}/src/unix/loop.c
  ${LIBUVDIR}/src/unix/pipe.c
  ${LIBUVDIR}/src/unix/poll.c
  ${LIBUVDIR}/src/unix/process.c
  ${LIBUVDIR}/src/unix/signal.c
  ${LIBUVDIR}/src/unix/stream.c
  ${LIBUVDIR}/src/unix/tcp.c
  ${LIBUVDIR}/src/unix/thread.c
  ${LIBUVDIR}/src/unix/timer.c
  ${LIBUVDIR}/src/unix/tty.c
  ${LIBUVDIR}/src/unix/udp.c)

add_library(uv STATIC ${SOURCES})

target_link_libraries(uv pthread curl)

SET( INC_DIRECTORY /usr/local/include)

include_directories(${INC_DIRECTORY})

set(SOURCE_FILES
    http_parser.c
    http_parser.h
    ${INC_DIRECTORY}/uv.h
    webserver.cc)

include_directories(.)


add_executable(TempServer ${SOURCE_FILES})

target_link_libraries(TempServer libuv.a)
target_link_libraries(TempServer curl)