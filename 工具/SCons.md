# 介绍

SCons是一种基于Python的自动化构建工具，类似于GNU Make。 它采用不同于通常Makefile文件的方式，而是使用SConstruct和SConscript文件来替代。 SCons是一个开放源代码、以Python语言编写的下一代程序建造工具，功能上类似于make。

# 安装 

## windows

1. 安装python：https://www.python.org/downloads/

2. `scons`官方并没有提供windows的安装程序，建议使用pip安装使用 `pip install scons` 命令安装 `scons`

3. `scons --version` 查看命令是否安装成功

# 简单使用

接下来我们就使用 `scons` 来编译程序，首先需要在项目目录下创建一个名为 `SConstruct` 的文件。

目录下的文件 ↓
```
main.cpp
SConstruct
```
SConstruct 文件内容 ↓ 
```python
Program('AppName', 'main.cpp')
```
在终端输入 `scons` 命令就可以编译程序了

```shell
# 编译
scons 

# 精简输出编译信息
scons -Q

# 编译并运行
scons ; .\AppName.exe
```
到这为止我们的程序就编译完成了然后就能在目录下看到名为 `AppName.exe` 的可执行文件。

## 清除

使用以下命令可以清除上一次编译的结果
``` shell
scons -c 
scons --clean
scons --remove
```

## 多文件编译

目前我们有3份源文件文件分别是 `main.cpp` `hello_world.hpp` `hello_world.cpp`

`main.cpp`

```cpp
#include "hello_world.hpp"

int main()
{
    print_hello_world();
}
```
`hello_world.hpp`

```cpp
#pragma once

#include <iostream>

void print_hello_world();
```

`hello_world.cpp`

```cpp
# include "hello_world.hpp"

void print_hello_world()
{
    std::cout << "hello world";
}
```

然后我们修改一下 `SConstruct.py` 文件中的内容

```python
Program('AppTest', ['main.cpp', 'hello_world.cpp'])
```
最后使用 `scons -Q` 命令就能顺利编译我们的文件了。

## Glob

如果你觉得一个个写入源文件的名称太麻烦了也可以使用 `Glob` 函数，这样 SCons 就会自动搜索源文件进行编译。

```python
Program('AppTest', Glob('*.cpp'))
```

## 更容易阅读的文件列表

`Split` 函数可以将字符串分割成列表，这样更加方便我们阅读与添加或者删除源文件。

```py
sources = Split("""
                    main.cpp
                    hello_world.cpp
                """)
Program('AppName', sources)
```

## 关键参数

target 指定输出目标名称

source 指定源文件

```py
sources = Split("""
                    main.cpp
                    hello_world.cpp
                """)

Program(target = 'AppName' , source = sources  )
Program(source = sources   , target = 'AppName')
```

最后使用 `scons -Q ; .\AppName.exe` 编译并运行程序

