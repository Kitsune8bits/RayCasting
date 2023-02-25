# Root CMake (equivalent of sln for msvc)
#
cmake_minimum_required(VERSION 3.22)

if(WIN32)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

# C/C++ standard
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Compile commands for LSP
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

IF( EXISTS "${CMAKE_CURRENT_BINARY_DIR}/compile_commands.json" )
    EXECUTE_PROCESS( COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${CMAKE_CURRENT_BINARY_DIR}/compile_commands.json
        ${CMAKE_CURRENT_SOURCE_DIR}/compile_commands.json
        )
ENDIF()

# Compiler flags
if(MSVC)
    add_compile_options(/W4 /WX /permissive-)
else()
    add_compile_options(-Wall -Wextra -Werror -pedantic)
endif()

# Project files
file(GLOB ROOT_SRC *.c *.cpp)

set(PROJECT_SRC ${ROOT_SRC} # Add project files here
    )

# Getting the project name from the directory
get_filename_component(PROJECTID ${CMAKE_CURRENT_SOURCE_DIR} NAME)
string(REPLACE " " "_" PROJECTID ${PROJECTID})

# Project settings
project(${PROJECTID}
    VERSION 0.0.0.0
    )

# Binary settings
add_executable(${PROJECTID} ${PROJECT_SRC})
target_include_directories(${PROJECTID} PUBLIC ${CMAKE_CURRENT_LIST_DIR}/)
