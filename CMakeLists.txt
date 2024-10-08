# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(FAMILY rp2040)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NEVER EDIT THE NEXT LINES for Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.0.0)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
include(${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)
# include($ENV{PICO_SDK_PATH}/lib/tinyusb/hw/bsp/family_support.cmake)

project(fidelio C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1
set(SOURCE_FILES 
        src/fidelio.c 
        src/rand.c 
        src/cert.c 
        src/u2f.c 
        src/usb_descriptors.c
        
        lib/wolfssl/wolfcrypt/src/asn.c
        lib/wolfssl/wolfcrypt/src/wc_port.c
        lib/wolfssl/wolfcrypt/src/wolfmath.c
        lib/wolfssl/wolfcrypt/src/hash.c
        lib/wolfssl/wolfcrypt/src/memory.c
        lib/wolfssl/wolfcrypt/src/misc.c
        lib/wolfssl/wolfcrypt/src/ecc.c
        lib/wolfssl/wolfcrypt/src/hmac.c
        lib/wolfssl/wolfcrypt/src/coding.c
        lib/wolfssl/wolfcrypt/src/random.c
        lib/wolfssl/wolfcrypt/src/sha256.c
        lib/wolfssl/wolfcrypt/src/sha512.c
        lib/wolfssl/wolfcrypt/src/ge_low_mem.c
        lib/wolfssl/wolfcrypt/src/fe_low_mem.c
        lib/wolfssl/wolfcrypt/src/ge_operations.c
        lib/wolfssl/wolfcrypt/src/sp_int.c
        lib/wolfssl/wolfcrypt/src/sp_armthumb.c
        lib/wolfssl/wolfcrypt/src/sp_c32.c
 
    
        )

add_executable(fidelio ${SOURCE_FILES})

pico_set_program_name(fidelio "Fidelio")
pico_set_program_version(fidelio "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(fidelio 0)
pico_enable_stdio_usb(fidelio 0)

# Add the standard library to the build
target_link_libraries(fidelio
        pico_stdlib
		pico_multicore
		hardware_adc
		hardware_flash
        pico_unique_id 
        tinyusb_device 
        tinyusb_board
        )

# Add the standard include files to the build
target_include_directories(fidelio PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/src
  ${CMAKE_CURRENT_SOURCE_DIR}/lib/wolfssl
)

target_compile_definitions(fidelio PUBLIC WOLFSSL_USER_SETTINGS)
target_compile_definitions(fidelio PUBLIC USE_BOOTSEL_BUTTON)

pico_add_extra_outputs(fidelio)

message("End of CMakeLists.txt.....")

