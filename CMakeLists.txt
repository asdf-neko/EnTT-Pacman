cmake_minimum_required(VERSION 3.10)

set(CMAKE_TOOLCHAIN_FILE "C:/codeTool/vcpkg/scripts/buildsystems/vcpkg.cmake" 
    CACHE STRING "Vcpkg toolchain file")

project(EnTT_Pacman)

# 1. 只将 .cpp 源文件加入可执行目标
add_executable(pacman
    src/core/app.cpp
    src/core/factories.cpp
    src/core/game.cpp
    src/core/maze.cpp
    src/sys/can_move.cpp
    src/sys/change_ghost_mode.cpp
    src/sys/eat_dots.cpp
    src/sys/house.cpp
    src/sys/movement.cpp
    src/sys/player_ghost_collide.cpp
    src/sys/player_input.cpp
    src/sys/pursue_target.cpp
    src/sys/render.cpp
    src/sys/set_target.cpp
    src/util/sdl_load_texture.cpp
    src/util/sdl_quad_writer.cpp
    src/util/sprites.cpp
    src/main.cpp
)

# 2. 配置 SDL2 依赖
if(CMAKE_TOOLCHAIN_FILE)
    message(STATUS "Using vcpkg toolchain file: ${CMAKE_TOOLCHAIN_FILE}")
    find_package(SDL2 CONFIG REQUIRED)
    
    target_link_libraries(pacman
        PRIVATE
        $<TARGET_NAME_IF_EXISTS:SDL2::SDL2main>
        SDL2::SDL2
    )
else()
    message(STATUS "Finding SDL2 via module path")
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/modules/")
    find_package(SDL2 REQUIRED)

    target_include_directories(pacman PRIVATE "${SDL2_INCLUDE_DIR}")
    target_link_libraries(pacman PRIVATE "${SDL2_LIBRARY}")
endif()

# 3. 包含项目头文件路径
target_include_directories(pacman
    PRIVATE
    "${CMAKE_SOURCE_DIR}/third_party"
    "${CMAKE_SOURCE_DIR}/src"
)

# 4. 设置 C++17 标准
target_compile_features(pacman PRIVATE cxx_std_17)

# 5. Debug 模式下开启严格的编译警告
if(CMAKE_BUILD_TYPE MATCHES "[Dd][Ee][Bb][Uu][Gg]")
    target_compile_options(pacman PRIVATE -Wall -Wextra -Wpedantic)
endif()

# 【新增】在 Windows 下隐藏控制台窗口
if(WIN32)
    target_link_options(pacman PRIVATE -mwindows)
endif()

# 【新增】构建后自动拷贝 SDL2.dll 到可执行文件所在目录
add_custom_command(TARGET pacman POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        $<TARGET_FILE:SDL2::SDL2>
        $<TARGET_FILE_DIR:pacman>
    COMMENT "Copying SDL2.dll to output directory..."
)

# 【可选】如果你的环境还缺 GCC 底层库，可以取消下面两行的注释
# COMMAND ${CMAKE_COMMAND} -E copy_if_different "C:/codeTool/w64devkit/bin/libstdc++-6.dll" $<TARGET_FILE_DIR:pacman>
# COMMAND ${CMAKE_COMMAND} -E copy_if_different "C:/codeTool/w64devkit/bin/libwinpthread-1.dll" $<TARGET_FILE_DIR:pacman>