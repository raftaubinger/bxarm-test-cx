set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)

cmake_minimum_required(VERSION 3.30)

project(Tutorial LANGUAGES C)

set(TOOLKIT_DIR /opt/iarsystems/bxrl78fs/rl78)

add_executable(tutorial)

target_sources(tutorial PRIVATE tutorial.c)

target_compile_options(tutorial PRIVATE --no_wrap_diagnostics)

target_link_options(tutorial PRIVATE
  --semihosting
)

# Convert the ELF output to .hex
function(iar_elf_tool_hex TARGET)
  add_custom_command(TARGET ${TARGET} POST_BUILD COMMAND ${CMAKE_IAR_ELFTOOL} --silent --ihex $<TARGET_FILE:${TARGET}> $<IF:$<BOOL:$<TARGET_PROPERTY:${TARGET},OUTPUT_NAME>>,$<TARGET_PROPERTY:${TARGET},OUTPUT_NAME>,$<TARGET_PROPERTY:${TARGET},NAME>>.hex)
endfunction()

iar_elf_tool_hex(tutorial)

