# godot实战记录

这里主要是用来记录我实战中的一些方法和技巧方便会议，当然有些过时的方法如果我再次用到也会进行修改。

## 垂直同步

godot默认开着垂直同步，不需要的话可以在项目设置中（常规）中搜索 "vsync" 或 "垂直同步" 找到关闭选项。

## 准星制作

![图片](./Images/godot准星制作_1.webp)

一个十字准星一般由一个中心原点，和四条线组成。在godot节点中我们需要先创建一个 `Control` 节点来存储我们所有的UI控件。这是所有 GUI 控件的基类，根据其父控件调整其位置和大小。

1. 将`Control`节点命名为`UserInterface`，再把(Anchors Preset) 设置为 `整个矩形`，(Filter)设置为 `Pass`
    
    <center> ↓ </center>
    
    ![图片](./Images/godot准星制作_2.webp)

2. 第一步添加一个 `CenterContainer` 节点并命名为 `Reticle`，再给他添加个脚本叫做 `reticle`

    <center> ↓ </center>

    ```gd
    extends CenterContainer

    ## 准星半径
    @export var DOT_RADIUS : float = 1.0
    ## 准星颜色
    @export var DOT_COLOR : Color = Color.WHITE

    func _ready():
        pass

    func _process(delta):
        pass

    func _draw():
	    draw_circle(Vector2(0,0), DOT_RADIUS, DOT_COLOR)
    ```

    <center> ↑ </center>

    我们主要关注`_draw()`函数中的内容，我们使用了个 ` draw_circle` 函数绘制了一个圆形，并给他传入了 `坐标` `准星宽度` `准星颜色` 等我们已经提前创建好的参数，然后我们就能从检查器中设置参数。
    <br>

3. 然后在检查器中设置 `Reticle` 参数

    <center> ↓ </center>

    ![图片](./Images/godot准星制作_3.webp)

4. 创建四个 `Line2D` 节点，分别明明为 `Top` `Right` `Bottom` `Left`，这里我们主要就设置宽度和大小。

    ![图片](./Images/godot准星制作_4.webp)

    坐标 | Top | Right | Bottom | Left
    :--- | :---: | :---: | :---: | :---:
    0X | 0 | 5 | 0 | -5
    0Y | -5 | 0 | 5 | 0
    1X | 0 | 15 | 0 | -15
    1Y | -15 | 0 | 15 | 0

    到这为止我们我们的准星就创建好了。

# godot节点记录

这个板块用来记录实战中使用过的节点并解释其作用

## CenterContainer

CenterContainer节点是一种容器节点，它的主要作用是将其所有子控件以最小尺寸保持在其中心位置

## PanelContainer

在Godot引擎中，PanelContainer节点是一个容器节点，它确保其子控件位于一个StyleBox区域内。这个节点通常用于为控件提供轮廓或背景样式。

主要特点
继承关系：PanelContainer继承自Container < Control < CanvasItem < Node < Object。
用途：它可以用来为控件提供一个带有样式的背景，使得控件看起来更有层次感。
主题属性：PanelContainer的背景样式由StyleBox panel属性定义。
使用示例
你可以在Godot编辑器中通过以下步骤添加一个PanelContainer节点：

在场景树中右键点击，选择“添加子节点”。
搜索并选择“PanelContainer”。
你可以在PanelContainer中添加其他控件，如Button、Label等。

## MarginContainer

在Godot引擎中，MarginContainer节点是一个容器节点，它会在其子控件的各个边缘添加可调整的边距。

主要特点
继承关系：MarginContainer继承自Container < Control < CanvasItem < Node < Object。
用途：它用于在所有子控件周围添加边距，而不是在每个子控件周围单独添加边距。
主题属性：可以通过主题属性margin_*来控制边距的大小。
使用示例
你可以在Godot编辑器中通过以下步骤添加一个MarginContainer节点：

在场景树中右键点击，选择“添加子节点”。
搜索并选择“MarginContainer”。
在MarginContainer中添加其他控件，如Label、Button等。

## VBoxContainer

在Godot引擎中，VBoxContainer节点是一个容器节点，它会将其子控件垂直排列。

主要特点
继承关系：VBoxContainer继承自BoxContainer < Container < Control < CanvasItem < Node < Object。
用途：它用于将子控件按垂直方向排列，适用于需要纵向布局的场景。
自动调整：当子控件的最小尺寸发生变化时，VBoxContainer会自动重新排列这些控件。

# 函数

## Engine.get_frames_per_second()

在Godot引擎中，`Engine.get_frames_per_second()` 是一个方法，用于获取当前游戏的每秒帧数（FPS）。这个方法返回一个浮点数，表示每秒渲染的帧数。

### 示例

以下是一个简单的GDScript示例，展示如何使用 `Engine.get_frames_per_second()` 来显示FPS：

```gd
extends Label

func _process(delta):
    text = "FPS: " + str(Engine.get_frames_per_second())
```