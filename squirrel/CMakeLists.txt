set(SQUIRREL_SRC sqapi.cpp
                 sqbaselib.cpp
                 sqclass.cpp
                 sqcompiler.cpp
                 sqdebug.cpp
                 sqfuncstate.cpp
                 sqlexer.cpp
                 sqmem.cpp
                 sqobject.cpp
                 sqstate.cpp
                 sqtable.cpp
                 sqvm.cpp)

add_library(squirrel SHARED ${SQUIRREL_SRC})
install(TARGETS squirrel RUNTIME DESTINATION "${CMAKE_INSTALL_FULL_BINDIR}"
                         LIBRARY DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}"
                         ARCHIVE DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}")

if(NOT DEFINED DISABLE_STATIC)
  add_library(squirrel_static STATIC ${SQUIRREL_SRC})
  install(TARGETS squirrel_static ARCHIVE DESTINATION "${CMAKE_INSTALL_FULL_LIBDIR}")
endif()

if(DEFINED LONG_OUTPUT_NAMES)
  set_target_properties(squirrel PROPERTIES OUTPUT_NAME squirrel3)

  if(NOT DEFINED DISABLE_STATIC)
    set_target_properties(squirrel_static PROPERTIES OUTPUT_NAME squirrel3_static)
  endif()
endif()
