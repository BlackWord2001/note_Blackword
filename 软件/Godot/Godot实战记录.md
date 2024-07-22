# godot实战记录

这里主要是用来记录我实战中的一些方法和技巧方便会议，当然有些过时的方法如果我再次用到也会进行修改。

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

## 节点 - `CenterContainer`

CenterContainer节点是一种容器节点，它的主要作用是将其所有子控件以最小尺寸保持在其中心位置