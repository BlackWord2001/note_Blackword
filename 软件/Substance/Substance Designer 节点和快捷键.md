# 快捷键操作

按键|作用
:---|:---
<kbd>Ctrl</kbd>| 可以多选节点或者连接线
<kbd>Backspace</kbd>| 删除节点但是不断开连接线
<kbd>Alt</kbd>| 在链接上增加拐点
<kbd>Shift</kbd> | 按住可以使参数中的角度按照45°∠旋转
<kbd>Space</kbd> | 快捷搜索节点

# Function Nodes
## Atomic Nodes

节点名称|节点作用
:---|:---
Blur（模糊）| 模糊图案
Blend（混合）| 可以混合两种不同的颜色
Transformation 2D| 可以缩放图像大小和位置便宜等变换操作
# Texture Generators
## Patterns
节点名称|节点作用
:---|:---
Shape | 提供多种基础的形状 如：方形，圆形
Tile Sampler | 瓷砖，瓦片 等基础类型
Gradient Linear 1 | 渐变
Gradient Linear 2 | 渐变
Gradient Linear 3 | 渐变


#  节点具体使用

## Tile Sampler

节点入口

![image](./images/tile_sampler_0.jpg)

+ Pattern Input 1：形状灰度图输入
+ Scale Map Input ：缩放图输入
+ Displacement Map Input：高度图输入

### Tile Sampler节点入口用法

** Pattern Input 1**  
如果想使用 Pattern Input 1输入图案需要在  INSTANCE PARAMETERS → Pattern → *Pattern*  中把默认 `Square` 换成 `Pattern Input`。

