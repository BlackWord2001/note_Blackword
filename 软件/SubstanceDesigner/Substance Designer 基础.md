> 注：Substance Designer 每个版本的中文翻译都有变化，特别是节点名称的改变会导致认不出节点，目前文档使用的 Substance Designer 版本是12.4.0。



# 链接/导入
使用方法：资源管理器 -> 文件名称.sbs（右键）-> 链接/导入
+ 链接会关联本地文件当你本地图片或者模型做出修改后会对进行一个同步，但如果你把外部的图片删除或者移动位置SD内文件就会丢失。
+ 导入相当于把图像直接导入进工程文件，当外部文件被删除的时候他也还会在SD文件中。

# 节点

![image](./images/1.jpg)| ![image](./images/2.jpg)
|:-:|:-:
|橙色代表颜色数据|灰色代表非颜色数据

如果颜色数据不匹配线会变成红色，他们之间是不能随便连接的所以就需要用到两个转换的节点
![images](./images/3.jpg)

灰度转换 和 渐变映射
+ 灰度转换：把颜色转换为灰度
+ 渐变映射：把灰度转换为颜色

![images](./images/4.jpg)

## 管理节点

选择需要的节点右键可以添加注释和取景框
+ 取景框（Frame）：用来给节点进行分类
+ 注释（Comment）：用于描述节点
+ 图钉（Pin）：可以创建导航点，按F2就能在多个导航点中切换。

![images](./images/5.jpg)

## 节点参数
+ 基本参数：所有节点都有的通用参数
+ 实列参数：节点特有参数
+ 特定参数：节点特有参数

#  节点具体使用

## Tile Sampler

节点入口

![image](./images/tile_sampler_0.jpg)

+ Pattern Input 1：形状灰度图输入
+ Scale Map Input ：缩放图输入
+ Displacement Map Input：高度图输入

### Tile Sampler节点入口用法

**Pattern Input 1**  
如果想使用 Pattern Input 1输入图案需要在  INSTANCE PARAMETERS → Pattern → *Pattern*  中把默认 `Square` 换成 `Pattern Input`。

**Pattern Input 2**  
INSTANCE PARAMETERS → Pattern → Pattern Input Number 可以设置更多图案输入，在 Pattern Input Distribution（图像输入分布）这个选项中默认使用的是 `Random` 会随机排列第二个图案，我们想并排可以改成 `Pattern Number` ，如果并排不是你要的效果那么可以调整 INSTANCE PARAMETERS → Position → `Offset` 的值来让他们交叉。 
