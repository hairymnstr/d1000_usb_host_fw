cmake_minimum_required(VERSION 3.12)

set(PICO_SDK_PATH ${CMAKE_CURRENT_LIST_DIR}/libs/pico-sdk)
include(libs/pico-sdk/external/pico_sdk_import.cmake)

set(PROJ_NAME "tusb_host")

project(${PROJ_NAME})

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.3.0")
    message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.3.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

pico_sdk_init()

add_compile_options(-Wall -Wextra)

add_executable(${PROJ_NAME})

# Example source
target_sources(${PROJ_NAME} PUBLIC
        ${CMAKE_CURRENT_LIST_DIR}/hid_app.c
        ${CMAKE_CURRENT_LIST_DIR}/main.c
        ${CMAKE_CURRENT_LIST_DIR}/msc_app.c
        ${CMAKE_CURRENT_LIST_DIR}/cdc_app.c
        )

# Make sure TinyUSB can find tusb_config.h
target_include_directories(${PROJ_NAME} PUBLIC
        ${CMAKE_CURRENT_LIST_DIR})

target_link_libraries(${PROJ_NAME} PRIVATE
        pico_stdlib
        tinyusb_board
        tinyusb_host
        )

pico_enable_stdio_usb(${PROJ_NAME} 0)
pico_enable_stdio_uart(${PROJ_NAME} 1)

pico_add_extra_outputs(${PROJ_NAME})

