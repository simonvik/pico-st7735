# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# initalize pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
#set(PICO_SDK_PATH "/data/pico/pico-sdk")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(pico-st7735 C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# library configuration (override if necessary)
set(SPI_TFT_PORT "spi0" CACHE STRING "TFT spi port-name")
set(SPI_TFT_CS   "21"    CACHE STRING "TFT CS  pin number")
set(SPI_TFT_DC   "7"   CACHE STRING "TFT DC  pin number")
set(SPI_TFT_RST  "6"   CACHE STRING "TFT RST pin number")
set(TFT_OPTIONS TFT_ENABLE_BLACK TFT_ENABLE_RESET TFT_ENABLE_TEXT TFT_ENABLE_SHAPES
                TFT_ENABLE_ROTATE TFT_ENABLE_SCROLL TFT_ENABLE_ALL
            CACHE STRING "TFT options/functions")

# TFT options/functions. Complete list:
#  - TFT_ENABLE_BLACK
#  - TFT_ENABLE_GREEN
#  - TFT_ENABLE_RED
#  - TFT_ENABLE_GENERIC
#  - TFT_ENABLE_TEXT
#  - TFT_ENABLE_FONTS
#  - TFT_ENABLE_ALL
#  - TFT_ENABLE_RESET
#  - TFT_ENABLE_SCROLL
#  - TFT_ENABLE_SHAPES
#  - TFT_ENABLE_ROTATE
#  - TFT_ENABLE_BMP  (not implemented yet)

add_compile_definitions(SPI_TFT_PORT=${SPI_TFT_PORT})
add_compile_definitions(SPI_TFT_CS=${SPI_TFT_CS})
add_compile_definitions(SPI_TFT_DC=${SPI_TFT_DC})
add_compile_definitions(SPI_TFT_RST=${SPI_TFT_RST})
foreach(opt IN LISTS TFT_OPTIONS)
    message(">>> using: ${opt}")
    add_compile_definitions(${opt})
endforeach(opt)

#program configuration (TODO: move to separate project)

set(SPI_PORT "spi0" CACHE STRING "spi port-name")
set(SPI_RX   "9"   CACHE STRING "RX  pin number")
set(SPI_TX   "23"   CACHE STRING "TX  pin number")
set(SPI_SCK  "22"   CACHE STRING "SCK pin number")

add_compile_definitions(SPI_PORT=${SPI_PORT})
add_compile_definitions(SPI_RX=${SPI_RX})
add_compile_definitions(SPI_TX=${SPI_TX})
add_compile_definitions(SPI_SCK=${SPI_SCK})

# ENABLE_FONTS: set on command-line or select from
# VSCode-menu (see .vscode/cmake-variants.json)
if(ENABLE_FONTS)
  message(">>> Font test")
  add_compile_definitions(TFT_ENABLE_FONTS ENABLE_TEST9 ENABLE_TEST9A)
else()
  message(">>> Normal test")
  add_compile_definitions(ENABLE_TESTR ENABLE_TEST1 ENABLE_TEST1A
                           ENABLE_TEST2 ENABLE_TEST3 ENABLE_TEST4
                           ENABLE_TEST5 ENABLE_TEST6 ENABLE_TEST7)
endif()

# add preprocessor-constant DEBUG for Debug-builds
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  set(CMAKE_VERBOSE_MAKEFILE 1)
  add_compile_definitions(DEBUG)
else()
endif()

add_subdirectory(lib-st7735)
add_subdirectory(demo)


target_compile_definitions(
  lib-st7735
  PUBLIC
  PIN_TFT_RST=${SPI_TFT_RST}
  PIN_TFT_DC=${SPI_TFT_DC}
  PIN_TFT_CS=${SPI_TFT_CS}
  )

