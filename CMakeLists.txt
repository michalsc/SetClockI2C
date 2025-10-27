cmake_minimum_required(VERSION 3.14.0)
project(SetClockI2C VERSION 1.0.0)

include(cmake/verstring.cmake)
get_verstring(VERSTRING)

if(NOT TARGET i2c)
    add_subdirectory(i2c.library EXCLUDE_FROM_ALL)
endif()

add_executable(SetClockI2C
    src/main.c
)

target_link_options(SetClockI2C PRIVATE -ffreestanding -m68040 -nostdlib -nostartfiles -Wl,-e,__start)
target_compile_options(SetClockI2C PRIVATE -O3 -m68040 -fomit-frame-pointer)
target_compile_definitions(SetClockI2C PRIVATE VERSION_STRING="${VERSTRING}")
set_target_properties(SetClockI2C PROPERTIES LINKER_LANGUAGE C)
target_link_libraries(SetClockI2C i2c)

install(PROGRAMS ${CMAKE_CURRENT_BINARY_DIR}/SetClockI2C DESTINATION I2C/)
