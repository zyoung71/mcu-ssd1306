cmake_minimum_required(VERSION 3.13)

# initialize the SDK based on PICO_SDK_PATH
# note: this must happen before project()
include(pico_sdk_import.cmake)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
file(CREATE_LINK ${CMAKE_CURRENT_BINARY_DIR}/compile_commands.json ${CMAKE_CURRENT_BINARY_DIR}/../../../compile_commands.json SYMBOLIC)

project(ssd1306-example)

set(CMAKE_C_STANDARD 11)

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

# rest of your project

add_subdirectory(../../lib/mcu-platform/platforms/rp2040 build/mcu-platform-rp2040)

add_executable(ssd1306-example
	example.c
    ../../ssd1306.c
)

target_include_directories(ssd1306-example PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/../../
    ${CMAKE_CURRENT_LIST_DIR}/../
)

target_link_libraries(ssd1306-example
    pico_stdlib
    mcu-platform-interface
    mcu-platform-rp2040
)

pico_enable_stdio_usb(ssd1306-example 1) 
pico_enable_stdio_uart(ssd1306-example 0) 

# create map/bin/hex/uf2 file in addition to ELF.
pico_add_extra_outputs(ssd1306-example)

