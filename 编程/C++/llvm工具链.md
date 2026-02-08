# llvm工具

## clang-format
代码格式化工具

### 基本用法

生成基于 LLVM 风格的配置文件
```bash
clang-format -style=llvm -dump-config > .clang-format
```
其他预定义风格：
    - llvm, google, chromium, mozilla, webkit
    - file (使用项目中的 .clang-format 文件)
    - {key: value, ...} (直接指定配置)

### 简单的格式化
```bash
# 2. 格式化当前目录所有文件
clang-format -i *.cpp *.h
```


### 更安全的格式化

```bash
# 只格式化 src 和 include 目录
clang-format -i src/*.cpp include/*.h
# 或递归
clang-format -i -r src/ include/
```