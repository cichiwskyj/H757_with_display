cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
#

# Setup compiler settings
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)

# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

message("Build type: " ${CMAKE_BUILD_TYPE})

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Compiler options
set(STM32_MCU_FLAGS  "-mcpu=cortex-m7 -mfpu=fpv5-d16 -mfloat-abi=hard " )

#Linker options
set(STM32_LINKER_SCRIPT stm32h757xx_flash_CM7.ld)
set(STM32_LINKER_OPTION  )

# Set the project name
set(CMAKE_PROJECT_NAME H757_with_display_CM7)
project(${CMAKE_PROJECT_NAME})

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Include mx-generated
include("mx-generated.cmake")


# In order to use CMake for cross-compiling
set(CMAKE_CXX_COMPILER_FORCED "true")

# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined symbols
)

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Add include paths
target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined include paths
)

# Add sources to executable
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user sources here
)

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined library search paths
)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    is42s32800j_lib
    otm8009a_lib
)

# Remove wrong libob.a library dependency when using cpp files
list(REMOVE_ITEM CMAKE_C_IMPLICIT_LINK_LIBRARIES ob)

# Validate that STM32CubeMX code is compatible with C standard
if((CMAKE_C_STANDARD EQUAL 90) OR (CMAKE_C_STANDARD EQUAL 99))
    message(ERROR "Generated code requires C11 or higher")
endif()


###########
# Driver library for Eval board SDRAM, is42s32800j
###########
add_library(is42s32800j_lib OBJECT)
target_sources(is42s32800j_lib PRIVATE is42s32800j/is42s32800j.c)
target_include_directories(is42s32800j_lib 
    PUBLIC 
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/is42s32800j
) 

target_link_libraries(is42s32800j_lib
    stm32cubemx
    STM32_Drivers
    ${TOOLCHAIN_LINK_LIBRARIES}
)

###########
# Driver library for eval board LCD screen, otm8009a
###########

add_library(otm8009a_lib OBJECT)
target_sources(otm8009a_lib PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/otm8009a/otm8009a.c ${CMAKE_CURRENT_SOURCE_DIR}/otm8009a/otm8009a_reg.c)
# target_include_directories(otm8009a_lib 
#     PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}
# )
target_include_directories(otm8009a_lib 
    PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/otm8009a
) 

target_link_libraries(otm8009a_lib
    stm32cubemx
    STM32_Drivers
    ${TOOLCHAIN_LINK_LIBRARIES}
)
