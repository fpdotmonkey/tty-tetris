cmake_minimum_required(VERSION 2.6)

project(tty-tetris)

option(HOST_DEBUG "Log to console" OFF)
option(USE_STDOUT_FOR_IO_CON "Use simplified console output functions" OFF)
option(USE_SNPRINTF_FOR_IO_CON "Use stdlib snprintf instead of own version" OFF)

add_definitions(-pipe --std=gnu99 -ftabstop=4 -Wno-unused-function)
add_definitions(-Wall -Wextra -Wpedantic)

if(USE_STDOUT_FOR_IO_CON)
  add_definitions(-DUSE_STDOUT_FOR_IO_CON)
endif()

if(USE_SNPRINTF_FOR_IO_CON)
  add_definitions(-DUSE_SNPRINTF_FOR_IO_CON)
endif()

file(GLOB SOURCES *.c nano/*.c)

add_executable(tty-tetris ${SOURCES})

if(HOST_DEBUG)
  add_definitions(-O0 -g3)
else()
  add_definitions(
    -Os
    -Werror
    -fvisibility=hidden
    -ffast-math
    -fmerge-all-constants
    -ffunction-sections
    -fdata-sections
    -s)
  set_target_properties(tty-tetris PROPERTIES LINK_FLAGS -Wl,--gc-sections)
endif()

install(
  TARGETS tty-tetris
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)
