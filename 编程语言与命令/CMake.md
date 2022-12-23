# CMake笔记

**VSCode: CMake Tools** <br>
> cmake tools插件不需要配置launch.json就能调试

<kbd>SHIFT</kbd> + <kbd>F5</kbd> 运行但不调试 <br>
<kbd>Ctrl</kbd> + <kbd>F5</kbd> 调试并运行

### C++项目目录组织方式
+ 文件结构
    + 项目名/include/项目名/模块名.h
    + 项目名/src/模块名.cpp

推荐使用`target_include_directories`而不是`include_directories`
`include_directories(header-dir)` 是一个全局包含，向下传递。
什么意思呢？就是说如果某个目录的 CMakeLists.txt 中使用了该指令，
其下所有的子目录默认也包含了header-dir 目录。



**实列**
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