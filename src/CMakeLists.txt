include_directories("${PROJECT_SOURCE_DIR}/src")
include_directories("${PROJECT_BINARY_DIR}")
include(GenerateExportHeader)
add_library(edit_a STATIC editline.c fn_complete.c history.c)
add_library(edit SHARED editline.c fn_complete.c history.c libedit.def)
set_target_properties(edit PROPERTIES PREFIX "")
GENERATE_EXPORT_HEADER(edit
  BASE_NAME edit
  EXPORT_MACRO_NAME edit_EXPORT
  EXPORT_FILE_NAME edit_Export.h
  STATIC_DEFINE edit_BUILT_AS_STATIC
)
add_executable(edit_test libedit_test.c)
target_link_libraries(edit_test edit)
add_executable(edit_test_dll libedit_test_dll.c)
install (TARGETS edit edit_test edit_test_dll
  DESTINATION "${CMAKE_INSTALL_PREFIX}/bin")
install (TARGETS edit_a
  DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")
install (FILES editline/readline.h
  DESTINATION "${CMAKE_INSTALL_PREFIX}/include/editline")
string(TOUPPER "${CMAKE_BUILD_TYPE}" uppercase_CMAKE_BUILD_TYPE)
if (MSVC AND uppercase_CMAKE_BUILD_TYPE MATCHES "DEBUG")
  install (FILES ${CMAKE_CURRENT_BINARY_DIR}/Debug/edit_a.pdb
    DESTINATION "${CMAKE_INSTALL_PREFIX}/lib")
  install (FILES ${CMAKE_CURRENT_BINARY_DIR}/Debug/edit.pdb
    DESTINATION "${CMAKE_INSTALL_PREFIX}/bin")
endif()
