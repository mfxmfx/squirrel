set(SQSTDLIB_SRC sqstdaux.cpp
                 sqstdblob.cpp
                 sqstdio.cpp
                 sqstdmath.cpp
                 sqstdrex.cpp
                 sqstdstream.cpp
                 sqstdstring.cpp
                 sqstdsystem.cpp)

add_library(sqstdlib SHARED ${SQSTDLIB_SRC})
target_link_libraries(sqstdlib squirrel)
install(TARGETS sqstdlib RUNTIME DESTINATION "${CMAKE_INSTALL_FULL_BINDIR}"
                         LIBRARY DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}"
                         ARCHIVE DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}")

if(NOT DEFINED DISABLE_STATIC)
  add_library(sqstdlib_static STATIC ${SQSTDLIB_SRC})
  install(TARGETS sqstdlib_static ARCHIVE DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}")
endif()

if(DEFINED LONG_OUTPUT_NAMES)
  set_target_properties(sqstdlib PROPERTIES OUTPUT_NAME sqstdlib3)

  if(NOT DEFINED DISABLE_STATIC)
    set_target_properties(sqstdlib_static PROPERTIES OUTPUT_NAME sqstdlib3_static)
  endif()
endif()
