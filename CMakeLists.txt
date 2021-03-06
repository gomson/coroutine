cmake_minimum_required(VERSION 3.1)
project(coroutine)

set(CMAKE_CXX_STANDARD 11)
set(includedir "${CMAKE_SOURCE_DIR}/include/coroutine")
set(srcdir "${CMAKE_SOURCE_DIR}/src")

set(linux_sources
    ${srcdir}/backend/linux/context.cpp
)
set(linux_headers
    ${includedir}/backend/linux/context.hpp
)
set(sources
    ${srcdir}/sized_memory_block.cpp
)
set(headers
    ${includedir}/sized_memory_block.hpp
    ${includedir}/callback.hpp
)

add_library(coroutine
    ${sources}
    ${headers}
)

target_include_directories(coroutine PUBLIC "${CMAKE_SOURCE_DIR}/include/")

if(UNIX AND NOT APPLE)
    target_sources(coroutine PUBLIC ${linux_headers} ${linux_sources})
endif()

option(COROUTINE_BUILD_EXAMPLES "Build coroutine examples" ON)

if(COROUTINE_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

configure_file(
    "${CMAKE_SOURCE_DIR}/.ycm_extra_conf.py.in"
    "${CMAKE_SOURCE_DIR}/.ycm_extra_conf.py"
)
