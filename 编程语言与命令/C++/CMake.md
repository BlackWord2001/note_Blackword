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



**实列: 比较老暂时还没空修改**
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
file(GLOB SRC_FILES CONFIGURE_ DEPENDS
    ${PROJECT_SOURCE_DIR}/lib/glad/src/*.c
    ${PROJECT_SOURCE_DIR}/src/*.cpp)

# 拷贝依赖文件到build目录
file(COPY ${PROJECT_SOURCE_DIR}/shaders DESTINATION ${BUILD_RPATH}/)
file(COPY ${PROJECT_SOURCE_DIR}/asset DESTINATION ${BUILD_RPATH}/)

add_executable(${CMAKE_PROJECT_NAME} ${SRC_FILES})

# 连接依赖库
target_link_libraries(${CMAKE_PROJECT_NAME} ${PROJECT_SOURCE_DIR}/lib/glfw/lib-vc2022/glfw3.lib)
```

# 现代CMAKE

**从现在开始，如果在命令行操作cmake，请使用更方便的-B 和 --build 命令。**

+ `cmake -B build` 免去了先创建 build 目录再切换进去再指定源码目录的麻烦。

+ `cmake --build build` 统一了不同平台（linux 上会调用 make，Windows上会调用 devenv.exe）

---

`cmake -B build -DCMAKE_BUILD_TYPE=Release`   
↑设置构建模式为发布模式（开启全部优化）     
cmake -B build ← 第二次配置时没有 -D 参数，但是之前的 -D 设置的变量都会被保留

## CMAKE_BUILD_TYPE构建的类型  
CMAKE_BUILD_TYPE 是 CMake 中一个特殊变量，用于控制构建类型，他的值可以是：  
+ Debug 调试模式，完全不优化，生成调试信息，方便调试程序
+ Release 发布模式，优化程度最高，性能最佳，但是编译比Debug慢.
+ MinSizeRel最小体积发布，生成的文件比Release更小，不完全优化，减少二进制体积
+ RelWithDeblnfo带调试信息发布，生成的文件比Release更大，因为带有调试的符号信息
+ 默认情况下CMAKE_BUILD_TYPE 为空字符串，这时相当于Debug。



## 自动查找文件
使用 GLOB 自动查找当前目录下指定扩展名文件，实现批量添加源文件，但是建议加上 CONFIGURE_DEPENDS 选项，当添加新的源文件的时，自动更新变量

~~~CMAKE
add_executable(main)

file(GLOB source CONFIGURE_DEPENDS *.cpp *.hpp *.c *.h)

target_sources(main PUBLIC ${sources})
~~~

GLOB 也可以替换成 GLOB_RECURSE 这样就能自动包含所有子文件夹下的文件，但是 GLOB_RECURSE 也会出现把 build 目录里生成的临时.cpp文件也给加进来，当然这是有解决方案的只要把源码放在src目录下就就能解决这个问题

## PROJECT_x_DIR 和 CMAKE_CURRENT_x_DIR 的区别

+ `PROJECT_SOURCE_DIR` 表示最近一次调用 project 的 CMakeLists.txt 所在的源码目录。  
+ `CMAKE_CURRENT_SOURCE_DIR` 表示当前 CMakeLists.txt的源码根目录。  
+ `CMAKE_SOURCE_DIR` 表示最外层 CMakeLists.txt的源码根目录 。   
> 利益 `PROJECT_SOURCE_DIR` 可以实现从子模块里直接获得项目最外层目录的路径。    
不建议使用 `CMAKE_SOURCE_DIR`，那样会让你的项目无法被人作为子模块使用。
