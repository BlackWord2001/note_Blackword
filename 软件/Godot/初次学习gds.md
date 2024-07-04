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

## 变量2.0

假如我们在if语句内部声明了一个变量，那么我们只能在if语句内部使用这个变量。这被称为作用域，是初学者常犯错误的一个地方。

```gds
func _ready():
	if x > y:
		var health = 100 #在if语句下创建的变量
```

如果我们想创建一个在任何地方使用的变量就需要把他放在顶部再任何函数之外，这样就是一个脚本范围内的变量，可以在脚本的任何位置调用。

```gds
extends Node

var script_variable = 100 #在最上方创建变量

func _ready():
	pass
```

 在gdscript中创建变量并不需要像c++一样提前写好类型，所有类型的统一创建方式就是通过"var"，如下我们创建了两个变量一个为bool类型一个为int类型。

 ```gds
 func _ready():
	var godot_is_cool = true
	var coolness = 9001
 ```

我们甚至能通过重新赋值来改变变量的类型

 ```gds
 func _ready():
	var godot_is_cool = true
	var coolness = 9001
	coolness = true
 ```

 gds语言有四种类型的变量

 bool：表示true和false

 int：整数

 float：浮点数

 string：文本

 ### 类型转换

我们可以通过str函数把number转换成string类型然后加入text中

 ```
 extends Node

func _ready():
	var number = 42
	var text = "Meaning of life: " + str(number)
	print(text)
 ```

 输出内容如下：
 ```
 Meaning of life: 42
 ```

 或者我们也能把浮点数转换成整数

 ```
 extends Node

func _ready():
	var pi = 3.14
	print(int(pi))
 ```

 这样最终输出就为3，但是要注意他只是截断了小数点后面的数字，并不是四舍五入。所以就算输入的是3.9也只会得到3。

 ### Vector

 除了常见的int float bool string类型，还有我们在游戏引擎中常用的vector类，通常 Vector2 或 Vector2 用来表示一个位置坐标。

 ```gds
 var position = Vector3(3, -10, 5)
 ```

 我们也可以修改 Vector3 中 xyz 中其中的值比如：

 ```gds
 extends Node

func _ready():
	var position = Vector3(3, -10, 5)
	position.x += 2
	print(position)
 ```

 ### 静态类型

 在godot中我们一般使用的变量类型都是动态的，引擎会根据我们的赋值或重新赋值来重新推断变量的类型。但是这样做就会导致性能的下降，所以在godot中我们可以可以像c/c++那些语言一样手动设置语言的类型，并且无法重新赋值为其他类型的变量。

 ```gds
 var damage: int = 15
 ```

 或者可以使用自动推断类型但是推断出来的类型是静态的无法被改变。

 ```gds
 var damage := 15
 ```

如果我们尝试将 damage变量设置为比如字符串引擎就会报错。

### @export

当我们想要在godot引擎的检查器中编辑我们的变量可以在我们的变量前添加 `@export` 关键字

![图像](./Images/gds基础学习_3.webp)

code:

```gds
extends Node

@export var damage: int = 15

func _ready():
	print(damage)
```

当程序运行时打印的结果就是我们在检查器中设置的数值

![图像](./Images/gds基础学习_4.webp)

## 常量

凡是有阴就有阳，变量的反面就是常量。定义一个常量我们只需要 `const` 关键字，一般常量的名称使用全大写。

```gds
const GRAVITY = -9.81
```

当我们创建了这个常量就没办法像变量一样在程序运行途中来修改他。