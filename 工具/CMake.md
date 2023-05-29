# **VSCode: CMake Tools** <br>
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



⚠**实列: 比较老暂时还没空修改**
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
> 利用 `PROJECT_SOURCE_DIR` 可以实现从子模块里直接获得项目最外层目录的路径。    
不建议使用 `CMAKE_SOURCE_DIR`，那样会让你的项目无法被人作为子模块使用。

## 其他相关变量

PROJECT_SOURCE_DIR 👈当前项目源码路径(存放main.cpp的地方)   
PROJECT_BINARY_DIR 👈当前项目输出路径(存放main.exe的地方)

PROJECT_SOURCE_DIR 👈根目录源码路径(存放main.cpp的地方)   
PROJECT_BINARY_DIR 👈根目录输出路径(存放main.exe的地方)  

PROJECT_IS_TOP_LIVEL 👈 BOOL类型，表示当前项目是否是(最顶层的)根项目    
PROJECT_NAME 👈 当前项目名  
CMAKE_PROJECT_NAME 👈 根项目的项目名    

## 设置CXX语言于标准

👇 同时启用 c c++
~~~CMAKE
project(hellocmake LANGUAGES C CXX)
~~~

👇 设置 c++ 标准
``` cmake
# 设置 c++ 标准为 c++17 如果想用20就写20
set(CMAKE_CXX_STANDARD 17)
# 检测到编译器如果不支持 c++17 标准就会报错
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# 设置ON表示开启 GCC 特有特性，如果需要跨平台开发就选择OFF
set(CMAKE_CXX_EXTENSIONS OFF)

# 以上变量请设置在project之前
project(hellocmake LANGUAGES C CXX)
```

# CMake 模块
说到cmake，可能最先想到的就是CmakeLists.txt文件，但是在很多情况下，也会看到.cmake文件。也许，你会诧异，.cmake文件是干什么的，甚至会想.cmake文件是不是cmake的正统文件，而CMakeLists.txt并不是。

但其实，CMakeLists.txt才是cmake的正统文件，而.cmake文件是一个模块文件，可以被include到CMakeLists.txt中。

include指令一般用于语句的复用，也就是说，如果有一些语句需要在很多CMakeLists.txt文件中使用，为避免重复编写，可以将其写在.cmake文件中，然后在需要的CMakeLists.txt文件中进行include操作就行了。

include指令的结构为 ↓ 

```cmake
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>]
                      [NO_POLICY_SCOPE])
```
虽然，有不少的可选参数，但是一般情况下，都是直接写：

```cmake
include(file|module)
```

# 一些踩坑记录

## Vscode cmake-tools 无法使用clang编译

当在**vscode**使用`cmake-tools`插件的时，并且使用`clang`编译器提示如：

> Unable to determine what CMake generator to use. Please install or configure a preferred generator, or update settings.json, your Kit configuration or PATH variable. 

> 无法确定要使用的CMake生成器。请安装或配置首选生成器，或更新settings.json、Kit配置或PATH变量。

1. 遇到以上提示请检查是否有安装`ninja`构建系统，在终端输入 `ninja` 或 `ninja --version` 检查是否有安装该软件。

2. 如果没有请先安装`ninja`构建系统，linux下应该直接使用包管理工具安装即可，windows下需要去下载 `ninja` 的二进制安装包然后将其手动添加到环境变量。

    ninja官网：https://ninja-build.org/

3. 安装完后重启vscode，删除之前cmake生成的文件重新配置即可。

### 如果还是不行就尝试以下修改

（Ctrl + Shift + P），然后输入“CMake: Edit User-Local CMake Kits”并按Enter。这将打开一个JSON文件，其中包含有关您的CMake工具链的信息。您可以在此文件中添加以下内容：

```json
{
    "name": "Clang",
    "compilers": {
        "C": "clang",
        "CXX": "clang++"
    },
    "generator": "ninja"
}
```

## 在终端指令中使用cmake编译

> 在使用Ninja和clang之前请先按照他们两个软件并添加到系统环境变量中。

如果我们直接使用 `cmake -B build` 生成中还是使用MSBuild和MSVC编译器构建的。

```shell
PS C:\Users\black\Desktop\VimLearn\learn1> cmake -B build
-- Selecting Windows SDK version 10.0.22000.0 to target Windows 10.0.22621.
-- The C compiler identification is MSVC 19.36.32532.0
-- The CXX compiler identification is MSVC 19.36.32532.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.36.32532/bin/Hostx64/x64/cl.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: C:/Program Files/Microsoft Visual Studio/2022/Community/VC/Tools/MSVC/14.36.32532/bin/Hostx64/x64/cl.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (2.6s)
-- Generating done (0.0s)
-- Build files have been written to: C:/Users/black/Desktop/VimLearn/learn1/build
PS C:\Users\black\Desktop\VimLearn\learn1> rm -r build
PS C:\Users\black\Desktop\VimLearn\learn1> ls
```

这时候我们需要在这串命令后面加上 `-G` 参数如下：

```shell
# 使用 Ninja 构建系统
cmake -B build -GNinja
```

这时候就能发现 cmake 开始调用系统中的 `clang`和`clang++`

```shell
PS C:\Users\black\Desktop\VimLearn\learn1> cmake -B build -GNinja
-- The C compiler identification is Clang 16.0.4 with GNU-like command-line
-- The CXX compiler identification is Clang 16.0.4 with GNU-like command-line
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: C:/Program Files/LLVM/bin/clang.exe - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: C:/Program Files/LLVM/bin/clang++.exe - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.8s)
-- Generating done (0.0s)
-- Build files have been written to: C:/Users/black/Desktop/VimLearn/learn1/build
```

其他方法，如果cmake并没有使用clang（暂时还没测试过）
```shell
cmake -B build -GNinja -DCMAKE_C_COMPILER=clang -DCMAKE_CXX_COMPILER=clang++
```