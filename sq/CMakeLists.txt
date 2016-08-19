add_executable(sq sq.c)
set_target_properties(sq PROPERTIES LINKER_LANGUAGE C)
set_target_properties(sq PROPERTIES INSTALL_RPATH "${CMAKE_INSTALL_FULL_LIBDIR}")
target_link_libraries(sq squirrel sqstdlib)
install(TARGETS sq RUNTIME DESTINATION "${CMAKE_INSTALL_FULL_BINDIR}")

if(NOT DEFINED DISABLE_STATIC)
  add_executable(sq_static sq.c)
  set_target_properties(sq_static PROPERTIES LINKER_LANGUAGE C)
  target_link_libraries(sq_static squirrel_static sqstdlib_static)
  install(TARGETS sq_static RUNTIME DESTINATION "${CMAKE_INSTALL_FULL_BINDIR}")
endif()

if(DEFINED LONG_OUTPUT_NAMES)
  set_target_properties(sq PROPERTIES OUTPUT_NAME squirrel3)

  if(NOT DEFINED DISABLE_STATIC)
    set_target_properties(sq_static PROPERTIES OUTPUT_NAME squirrel3_static)
  endif()
endif()

if(CMAKE_COMPILER_IS_GNUCXX AND (NOT DEFINED DISABLE_STATIC))
  set_target_properties(sq_static PROPERTIES COMPILE_FLAGS "-static -Wl,-static")
endif()
