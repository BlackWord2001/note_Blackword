# gds学习

和其他游戏引擎编程一样都有如下两个函数作为我们编写脚本的位置。

```gds
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

```gds
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

```gds
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

## 变量1.0

我们可以创建一个变量对他进行运算，这和别的编程语言一样，最后我们再使用print打印结果。

```gds
extends Node

var health = 100

func _ready():
	
	health = 40
	health = 20 + 30
	health += 20
	health -= 10
	health *= 4
	health /= 2

	print(health)
```

假如我们正在制作一款游戏，每按下一次空格玩家生命值就会减少20。

```gds
extends Node

var health = 100

func _input(event):
	if event.is_action_pressed("my_action"): #当我们按下空格的时候
		health -= 20 #生命值减少20
		print(health) #打印生命值结果
```

## if语句

但是有一个问题当玩家的生命值会变为0后继续按空格生命值会变成负数，所以我们要解决这个问题。

```gds
extends Node

var health = 100

func _input(event):
	if event.is_action_pressed("my_action"): #当我们按下空格的时候
		health -= 20 #生命值减少20
		print(health) #打印生命值结果
		
		#当生命值小于或等于0的时候执行以下代码
		if health <= 0:
			health = 0
			print("YOU DIDE")
```

if语句还有非常多的用法

```
x == y # x小于y
```
```
x != y # x不等于y
```
```
x > y  # x大于y
```
```
x < y  # x小于y
```
```
x >= y # x大于等于y
```
```
x <= y # x小于等于y
```

我们还可以对if语句添加更多的条件

and关键字代表这两个条件都要满足
```
if x == y and y > z:
```

使用or关键字代表只需要满足其中一个
```
if x == y or y > z:
```

### else elif

else语句可以让我们实现条件为假时发生什么；
而elif可以在if和else中间再添加判断条件来扩展我们的if。

``` gds
extends Node

var health = 100

func _input(event):
	if event.is_action_pressed("my_action"): #当我们按下空格的时候
		health -= 20 #生命值减少20
		print(health) #打印生命值结果
		
		#当生命值小于或等于0的时候执行以下代码
		if health <= 0:
			health = 0
			print("你死了!")
		elif health < 50:
			print("你的生命值过低!")
		else:
			print("你的生命值很健康!")
```

