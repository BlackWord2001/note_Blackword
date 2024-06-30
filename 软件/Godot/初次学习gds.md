# gds学习

和其他游戏引擎编程一样都有如下两个函数作为我们编写脚本的位置。

```py
extends Node

# 当节点第一次进入场景树时调用。
func _ready():
	pass # pass基本等于此函数不会执行任何内容


# 调用每一帧。'delta'是自上一帧以来经过的时间。
func _process(delta):
	pass
```

## 语法

当我们创建了一个Label节点先随便添加一些文字内容，如果我们想通过代码修改Label中的内容或者颜色我们可以把鼠标放在具体的修改选项上看他的属性。

![图像](./Images/gds基础学习_0.webp)

```py
extends Node

# Called when the node enters the scene tree for the first time.
func _ready():
	print("hello world")
	$Label.text = "hello, world" # 修改Label的文本内容
	$Label.modulate = Color.GREEN # 修改文本颜色为绿色
```

运行效果

![图像](./Images/gds基础学习_1.webp)

## 输入

接下来我们需要做一个按下空格改变字体颜色的功能。首先先在输入映射中添加一个名为"my_action"的输入动作，并将他设置为空格键。

![图像](./Images/gds基础学习_2.webp)

然后回到我们的代码中

```py
extends Node

func _ready():
	print("hello world")
	$Label.text = "hello, world"
	$Label.modulate = Color.GREEN

# godot中监测按键输入的函数
func _input(event):
	if event.is_action_pressed("my_action"): # 检测是否按下
		$Label.modulate = Color.RED # 按下把文字改为红色
		
	if event.is_action_released("my_action"): # 检测是否放开
		$Label.modulate = Color.GREEN # 松开把文字修改为绿色
```

最终效果就是我们按下<kbd>space</kbd>后文字会变成红色，松开后变回绿色。