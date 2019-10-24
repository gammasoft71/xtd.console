cmake_minimum_required(VERSION 3.3)

# Project
project(xtd.console VERSION 0.0.1)
set(INCLUDES
  include/xtd/__generic_stream_output.hpp
  include/xtd/__get_err_rdbuf.hpp
  include/xtd/__get_in_rdbuf.hpp
  include/xtd/__get_out_rdbuf.hpp
  include/xtd/__opaque_console.hpp
  include/xtd/background_color.hpp
  include/xtd/basic_console.hpp
  include/xtd/beep.hpp
  include/xtd/console.hpp
  include/xtd/console_cancel_event_args.hpp
  include/xtd/console_cancel_event_handler.hpp
  include/xtd/console_color.hpp
  include/xtd/console_key.hpp
  include/xtd/console_key_info.hpp
  include/xtd/console_modifiers.hpp
  include/xtd/console_special_key.hpp
  include/xtd/foreground_color.hpp
  include/xtd/reset_color.hpp
  include/xtd/xtd.console
  include/xtd/xtd.console.hpp
)
set(SOURCES
  src/__opaque_console_gcc.cpp
  src/__opaque_console_win32.cpp
  src/console.cpp
)
source_group(include FILES ${INCLUDES})
source_group(src FILES ${SOURCES})

# Options
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Library properties
add_library(${PROJECT_NAME} STATIC ${INCLUDES} ${SOURCES})
if (MSVC)
  target_compile_options(${PROJECT_NAME} PRIVATE "$<$<CONFIG:Debug>:/Fd$<TARGET_FILE_DIR:${PROJECT_NAME}>/${PROJECT_NAME}${CMAKE_DEBUG_POSTFIX}.pdb>")
  target_compile_options(${PROJECT_NAME} PRIVATE "$<$<CONFIG:Release>:/Fd$<TARGET_FILE_DIR:${PROJECT_NAME}>/${PROJECT_NAME}.pdb>")
endif ()
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> PUBLIC $<INSTALL_INTERFACE:include>)
target_link_libraries(${PROJECT_NAME} xtd.delegates xtd.diagnostics xtd.core xtd.io xtd.strings)
set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER "xtd/src")

# install
install(DIRECTORY include/xtd/. DESTINATION include/xtd)
install(FILES $<TARGET_FILE_DIR:${PROJECT_NAME}>/${PROJECT_NAME}${CMAKE_DEBUG_POSTFIX}.pdb DESTINATION lib CONFIGURATIONS Debug OPTIONAL)
install(FILES $<TARGET_FILE_DIR:${PROJECT_NAME}>/${PROJECT_NAME}.pdb DESTINATION lib CONFIGURATIONS Release OPTIONAL)
install(TARGETS ${PROJECT_NAME} EXPORT ${EXPORT_PROJECT_NAME} DESTINATION lib)