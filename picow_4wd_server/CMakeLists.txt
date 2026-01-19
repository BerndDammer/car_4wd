# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.2.0)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.2.0-a4)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(picow_4wd_server C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

###########################################################################################

include_directories(
        ${PROJECT_SOURCE_DIR}/include
        ${PROJECT_SOURCE_DIR}/src
)

file(GLOB all_SRCS
        "${PROJECT_SOURCE_DIR}/include/*.h"
        "${PROJECT_SOURCE_DIR}/include/*.hpp"
        "${PROJECT_SOURCE_DIR}/src/*.cpp"
        "${PROJECT_SOURCE_DIR}/src/*.c"
        "${PROJECT_SOURCE_DIR}/src/*.S"
)


add_executable(picow_4wd_server ${all_SRCS})

###########################################################################################
include(FreeRTOS_Kernel_import.cmake)

# target_include_directories(picow4wd_server PRIVATE freertos/include freertos/portable/ThirdParty/GCC/RP2040/include/)

# don't want NO_SYS (generally this would be in your lwipopts.h)
target_compile_definitions(picow_4wd_server PRIVATE NO_SYS=0 )

#####################################################################################


pico_set_program_name(picow_4wd_server "picow_4wd_server")
pico_set_program_version(picow_4wd_server "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(picow_4wd_server 1)
pico_enable_stdio_usb(picow_4wd_server 1)

# Add the standard library to the build
target_link_libraries(picow_4wd_server
        pico_stdlib)

# Add the standard include files to the build
target_include_directories(picow_4wd_server PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
)

# Add any user requested libraries
target_link_libraries(picow_4wd_server 
        hardware_spi
        hardware_timer
        hardware_clocks
        hardware_pwm
        pico_cyw43_arch_lwip_sys_freertos
        FreeRTOS-Kernel-Heap4
)

pico_add_extra_outputs(picow_4wd_server)

