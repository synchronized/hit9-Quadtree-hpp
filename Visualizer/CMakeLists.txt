cmake_minimum_required(VERSION 3.10)

project(QuadtreeVisualizer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

# windows 动态库导出lib文件
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

# 设置VisualStudio使用utf-8编码
add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")

# 设置可执行文件和dll文件的输出目录
set(publish_bin_debug 			${CMAKE_CURRENT_SOURCE_DIR}/bin/debug)
set(publish_bin_release 		${CMAKE_CURRENT_SOURCE_DIR}/bin/release)

set(PROJECT_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${publish_bin_debug})
set(PROJECT_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${publish_bin_release})


file(GLOB SOURCES *.cpp)
add_executable(QuadtreeVisualizer ${SOURCES})

find_package(SDL2 CONFIG REQUIRED)
find_package(spdlog CONFIG REQUIRED)
find_package(argparse CONFIG REQUIRED)

include_directories("../Source")

target_link_libraries(QuadtreeVisualizer
        PRIVATE
        $<TARGET_NAME_IF_EXISTS:SDL2::SDL2main>
        $<IF:$<TARGET_EXISTS:SDL2::SDL2>,SDL2::SDL2,SDL2::SDL2-static>
    )
target_link_libraries(QuadtreeVisualizer PRIVATE spdlog::spdlog)
target_link_libraries(QuadtreeVisualizer PRIVATE argparse::argparse)
