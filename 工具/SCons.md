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
scons
```
到这为止我们的程序就编译完成了然后就能在目录下看到名为 `AppName.exe` 的可执行文件。