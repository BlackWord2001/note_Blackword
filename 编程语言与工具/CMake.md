# CMake笔记

``` cmake
#设置CMake最低版本号
cmake_minimum_required(VERSION 3.20)

#项目名称
project(glStudy)

#包含目录
include_directories(
    ${PROJECT_SOURCE_DIR}/lib
    ${PROJECT_SOURCE_DIR}/lib/glad/include
    ${PROJECT_SOURCE_DIR}/lib/glfw/include
    ${PROJECT_SOURCE_DIR}/include)

#源码目录 保存到名为"SRC_FILES"的变量
file(GLOB SRC_FILES
    ${PROJECT_SOURCE_DIR}/lib/glad/src/*.c
    ${PROJECT_SOURCE_DIR}/src/*.cpp)

# 拷贝依赖文件到build目录
file(COPY ${PROJECT_SOURCE_DIR}/shaders DESTINATION ${BUILD_RPATH}/)
file(COPY ${PROJECT_SOURCE_DIR}/asset DESTINATION ${BUILD_RPATH}/)

add_executable(${CMAKE_PROJECT_NAME} ${SRC_FILES})

# 连接依赖库
target_link_libraries(${CMAKE_PROJECT_NAME} ${PROJECT_SOURCE_DIR}/lib/glfw/lib-vc2022/glfw3.lib)
```