cmake_minimum_required(VERSION 3.24)
set(CMAKE_TOOLCHAIN_FILE ${CMAKE_SOURCE_DIR}/ARMCC.cmake)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

project(ms C)

if (NOT WIN32)
    set_compilers()
endif()

if (WIN32)
    set(LOCALE_FLAG --locale=japanese)
else()
    set(LOCALE_FLAG --locale=ja_JP.SJIS)
endif()

__compiler_armcc(C)


file(GLOB_RECURSE SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c)

add_library(ms ${SOURCES})

target_include_directories(ms PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_compile_options(ms PUBLIC
    --apcs=//interwork --arm
    --cpu=MPCore --fpmode=fast -Otime
    --data-reorder --split_sections
    --multibyte-chars --signed_chars
    ${LOCALE_FLAG}
)
target_compile_options(ms PUBLIC $<$<COMPILE_LANGUAGE:C>:--c99>)
