if(MSVC)
  cmake_minimum_required(VERSION 3.4)
else()
  cmake_minimum_required(VERSION 2.8.5)
endif()

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "" FORCE)
endif()
if(NOT CMAKE_INSTALL_PREFIX)
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/Install" CACHE PATH "" FORCE)
endif()

project(squirrel C CXX)
include(GNUInstallDirs)

include_directories(${CMAKE_SOURCE_DIR}/include)

if(CMAKE_C_COMPILER_ID MATCHES "^(Clang|GNU)$")
  set(SQ_FLAGS -fno-exceptions -fno-strict-aliasing)
  set(SQ_FLAGS ${SQ_FLAGS} -Wall -Wextra -Wcast-qual -pedantic)
  if(ENABLE_WERROR)
    message(STATUS "Info: using '-Werror'")
    set(SQ_FLAGS ${SQ_FLAGS} -Werror)
  endif()

  # FIXME: why do we set these flags here?? It is cmake's job to determine
  # the proper settings for the detected compiler, and you can always override
  # on the command line by using something like
  #   cmake .. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_C_FLAGS_DEBUG="-g3 -O0" -DCMAKE_CXX_FLAGS_DEBUG="-g3 -O0"
  if(0) # FIXME
  if(CMAKE_BUILD_TYPE STREQUAL "Release")
    set(SQ_FLAGS ${SQ_FLAGS} -O3)
  elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    set(SQ_FLAGS ${SQ_FLAGS} -O3 -g)
  elseif(CMAKE_BUILD_TYPE STREQUAL "MinSizeRel")
    set(SQ_FLAGS ${SQ_FLAGS} -Os)
  elseif(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(SQ_FLAGS ${SQ_FLAGS} -g3 -O0)
  endif()
  endif()

  message(STATUS "Info: using SQ_FLAGS=${SQ_FLAGS}")
  if(CMAKE_VERSION VERSION_GREATER 3)
    add_compile_options(${SQ_FLAGS})
  else()
    add_definitions(${SQ_FLAGS})
  endif()

  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-rtti -std=c++0x") # C++11 for "long long"
elseif(MSVC)
  set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  add_definitions(-D_SQ64)
endif()

add_subdirectory(squirrel)
add_subdirectory(sqstdlib)
add_subdirectory(sq)

if(NOT WIN32)
  set_target_properties(squirrel sqstdlib PROPERTIES SOVERSION 0 VERSION 0.0.0)
endif()

set(SQ_PUB_HEADERS include/sqconfig.h
                   include/sqstdaux.h
                   include/sqstdblob.h
                   include/sqstdio.h
                   include/sqstdmath.h
                   include/sqstdstring.h
                   include/sqstdsystem.h
                   include/squirrel.h)
install(FILES ${SQ_PUB_HEADERS} DESTINATION "${CMAKE_INSTALL_FULL_INCLUDEDIR}/squirrel")
