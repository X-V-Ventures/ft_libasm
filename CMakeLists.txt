cmake_minimum_required(VERSION 3.18)

project(ft_libasm)

option(Debug "Build with debugging information" OFF)

target_include_directories(ft_libasm PRIVATE ${PROJECT_SOURCE_DIR}/inc)
file(GLOB_RECURSE SOURCES "src/*.s")

enable_language(ASM_NASM)
set_source_files_properties(${SOURCES} PROPERTIES LANGUAGE ASM_NASM)

add_library(ft_libasm STATIC ${SOURCES})
set_target_properties(ft_libasm PROPERTIES OUTPUT_NAME "ft_libasm")

if(Debug)
    target_compile_definitions(ft_libasm PRIVATE DEBUG=1)
    set_target_properties(ft_libasm PROPERTIES COMPILE_OPTIONS "-g;-Fdwarf")
    message(STATUS "Building with debugging information")
endif()

add_custom_target(fclean
    COMMAND ${CMAKE_COMMAND} -E remove_directory ${CMAKE_BINARY_DIR}
    COMMENT "Removing the build directory and all build artifacts"
)

add_custom_target(re
    COMMAND ${CMAKE_COMMAND} --build . --target fclean
    COMMAND ${CMAKE_COMMAND} --build . --target all
    COMMENT "Rebuilding the project"
)
