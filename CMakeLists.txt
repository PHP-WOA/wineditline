cmake_minimum_required (VERSION 3.15)
project (WinEditLine)
set (WinEditLine_VERSION_MAJOR 2)
set (WinEditLine_VERSION_MINOR 2)
if (MSVC AND MSVC_USE_STATIC_RUNTIME)
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
if(NOT DEFINED LIB_SUFFIX)
  if(CMAKE_SIZEOF_VOID_P MATCHES 4)
    set(LIB_SUFFIX "32")
  else()
    set(LIB_SUFFIX "64")
  endif()
endif()
configure_file (
  "${PROJECT_SOURCE_DIR}/src/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
)
add_subdirectory (src)
