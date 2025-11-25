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
# ============================================================
#   Generated CMake Pico project file (final merged version)
# ============================================================

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# == DO NOT EDIT: Required by Raspberry Pi Pico VS Code Extension ==
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
# ==================================================================

set(PICO_BOARD pico_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(distance_vector C CXX ASM)

# Initialize the SDK
pico_sdk_init()

# ============================================================
#       SOURCE AND INCLUDE PATHS
# ============================================================

set(SourceList
    main.c
    connect.c
    distance_vector.c
    layout.c
    node.c
    packet.c
    utils.c
    wifi_scan.c
    dhcpserver/dhcpserver.c
)

include_directories(
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/dhcpserver
    ${CMAKE_CURRENT_LIST_DIR}/lwipopts
    ${CMAKE_CURRENT_LIST_DIR}/protothreads
)

# Common libraries
set(DV_LIBS
    pico_cyw43_arch
    pico_cyw43_arch_lwip_threadsafe_background
    pico_stdlib
    pico_multicore
)

# ============================================================
#       EXECUTABLES
# ============================================================

# -------------------- dv_master --------------------
add_executable(dv_master ${SourceList})
pico_enable_stdio_usb(dv_master 1)
pico_enable_stdio_uart(dv_master 0)
target_compile_definitions(dv_master PRIVATE MASTER)
target_link_libraries(dv_master PRIVATE ${DV_LIBS})
pico_add_extra_outputs(dv_master)

# -------------------- dv_node_UART --------------------
add_executable(dv_node_UART ${SourceList})
pico_enable_stdio_usb(dv_node_UART 0)
pico_enable_stdio_uart(dv_node_UART 1)
target_link_libraries(dv_node_UART PRIVATE ${DV_LIBS})
pico_add_extra_outputs(dv_node_UART)

# -------------------- dv_node_USB --------------------
add_executable(dv_node_USB ${SourceList})
pico_enable_stdio_usb(dv_node_USB 1)
pico_enable_stdio_uart(dv_node_USB 0)
target_compile_definitions(dv_node_USB PRIVATE SERIAL_OVER_USB)
target_link_libraries(dv_node_USB PRIVATE ${DV_LIBS})
pico_add_extra_outputs(dv_node_USB)

# -------------------- dv_LAYOUT_master --------------------
add_executable(dv_LAYOUT_master ${SourceList})
pico_enable_stdio_usb(dv_LAYOUT_master 1)
pico_enable_stdio_uart(dv_LAYOUT_master 0)
target_compile_definitions(dv_LAYOUT_master PRIVATE MASTER USE_LAYOUT)
target_link_libraries(dv_LAYOUT_master PRIVATE ${DV_LIBS})
pico_add_extra_outputs(dv_LAYOUT_master)

# -------------------- dv_LAYOUT_node_UART --------------------
add_executable(dv_LAYOUT_node_UART ${SourceList})
pico_enable_stdio_usb(dv_LAYOUT_node_UART 0)
pico_enable_stdio_uart(dv_LAYOUT_node_UART 1)
target_compile_definitions(dv_LAYOUT_node_UART PRIVATE USE_LAYOUT)
target_link_libraries(dv_LAYOUT_node_UART PRIVATE ${DV_LIBS})
pico_add_extra_outputs(dv_LAYOUT_node_UART)

# -------------------- dv_LAYOUT_node_USB --------------------
add_executable(dv_LAYOUT_node_USB ${SourceList})
pico_enable_stdio_usb(dv_LAYOUT_node_USB 1)
pico_enable_stdio_uart(dv_LAYOUT_node_USB 0)
target_compile_definitions(dv_LAYOUT_node_USB PRIVATE USE_LAYOUT SERIAL_OVER_USB)
target_link_libraries(dv_LAYOUT_node_USB PRIVATE ${DV_LIBS})
pico_add_extra_outputs(dv_LAYOUT_node_USB)

# Compiler optimization
add_compile_options(-O2)

# ============================================================
#   END OF FILE
# ============================================================
