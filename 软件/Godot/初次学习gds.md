# gds学习

和其他游戏引擎编程一样都有如下两个函数作为我们编写脚本的位置。

```gd
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

```gd
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

```gd
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

```gd
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

```gd
extends Node

var health = 100

func _input(event):
	if event.is_action_pressed("my_action"): #当我们按下空格的时候
		health -= 20 #生命值减少20
		print(health) #打印生命值结果
```

## if语句

但是有一个问题当玩家的生命值会变为0后继续按空格生命值会变成负数，所以我们要解决这个问题。

```gd
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

``` gd
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

```gd
func _ready():
	if x > y:
		var health = 100 #在if语句下创建的变量
```

如果我们想创建一个在任何地方使用的变量就需要把他放在顶部再任何函数之外，这样就是一个脚本范围内的变量，可以在脚本的任何位置调用。

```gd
extends Node

var script_variable = 100 #在最上方创建变量

func _ready():
	pass
```

 在gdscript中创建变量并不需要像c++一样提前写好类型，所有类型的统一创建方式就是通过"var"，如下我们创建了两个变量一个为bool类型一个为int类型。

 ```gd
 func _ready():
	var godot_is_cool = true
	var coolness = 9001
 ```

我们甚至能通过重新赋值来改变变量的类型

 ```gd
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

 ```gd
 var position = Vector3(3, -10, 5)
 ```

 我们也可以修改 Vector3 中 xyz 中其中的值比如：

 ```gd
 extends Node

func _ready():
	var position = Vector3(3, -10, 5)
	position.x += 2
	print(position)
 ```

 ### 静态类型

 在godot中我们一般使用的变量类型都是动态的，引擎会根据我们的赋值或重新赋值来重新推断变量的类型。但是这样做就会导致性能的下降，所以在godot中我们可以可以像c/c++那些语言一样手动设置语言的类型，并且无法重新赋值为其他类型的变量。

 ```gd
 var damage: int = 15
 ```

 或者可以使用自动推断类型但是推断出来的类型是静态的无法被改变。

 ```gd
 var damage := 15
 ```

如果我们尝试将 damage变量设置为比如字符串引擎就会报错。

### @export

当我们想要在godot引擎的检查器中编辑我们的变量可以在我们的变量前添加 `@export` 关键字

![图像](./Images/gds基础学习_3.webp)

code:

```gd
extends Node

@export var damage: int = 15

func _ready():
	print(damage)
```

当程序运行时打印的结果就是我们在检查器中设置的数值

![图像](./Images/gds基础学习_4.webp)

## 常量

凡是有阴就有阳，变量的反面就是常量。定义一个常量我们只需要 `const` 关键字，一般常量的名称使用全大写。

```gd
const GRAVITY = -9.81
```

当我们创建了这个常量就没办法像变量一样在程序运行途中来修改他。

## 函数

我们可以用 `func` 来创建一个名为“JUMP”的函数，然后再把上次绑定的空格键作为我们的触发方式来看看是否起效果。

```gd
extends Node

func _input(event):
	if event.is_action_pressed("my_action"):
		jump()

func jump():
	print("JUMP!")
```

在代码中我们给函数提供的输入称为参数，输出称为返回值。如下代码我们要创建带两个参数的一个函数即我们想相加的数字，但函数其实并没有返回结果只是打印了他。

```gd
extends Node

func _ready():
	add(245, 111)

func add(num1, num2):
	var result = num1 + num2
	print(result)
```

想要返回值的话我们只需要把 `print()` 替换成 `return` 就可以了，然后在`_ready` 函数中创建一个变量用来存储 `add()` 相加出来的值再传递给 `print()` 进行打印。

```gd
extends Node

func _ready():
	var result = add(245, 111)
	print(result)

func add(num1, num2):
	var result = num1 + num2
	return result
```

甚至还能套娃使用 `add()` 让自身相加，或者直接在 `print()` 中使用 `add()`。

```gd
var result = add(245, 111)
result = add(result, 10)
print(result)
print(add(10, 10))
```

函数中的参数我们也可以像创建变量一样指定数据类型，如下

```gd
func add(num1: int, num2: int):
	var result = num1 + num2
	return result
```

我们甚至还能加上 `-> int` 来限制我们的返回值类型

```gd
func add(num1: int, num2: int) -> int:
	var result = num1 + num2
	return result
```

## 随机数

我们可以使用 `randf()` 创建随机数，假设我们在写一套关于玩家掉落物概率的随机代码，判断当我们的随机生成的数值大于等于0.8的时候掉落的是稀有物品，否则则是普通物品。

`randf()` 返回 0.0 和 1.0（包含）之间的随机浮点值，除了`randf()`还有`randi()` 如果你经常使用其他编程语言你可能已经发现了，f其实是float的缩小而i是int的缩写，不同的随机数函数可以指定生成不同的数据类型。

```gd
extends Node

func _ready():
	var roll = randf()
	print(roll)
	
	if roll <= 0.8:
		print("你获得了一件普通物品。")
	else:
		print("你获得了一件稀有物品！")
```

接下来我们要给随机数定一个随机范围，假如我要随机随机一名角色的身高我会使用140cm-210cm作为随机数生成的范围。

`randi_range` 就是可以设置随机范围的随机数生成函数，括号中可以填两个参数前面的是最小值后面的是最大值。

```gd
extends Node

func _ready():
	var character_height = randi_range(140, 210)
	print("你的身高是：" + str(character_height) + "cm。")
```

## 引擎内文档

godot引擎在引擎内置了文档，当你想查看某个引擎内置函数的使用方法的时候就可以长按 <kbd>Ctrl</kbd> + 鼠标左键单击跳转到相应的文档。

![图像](./Images/gds基础学习_5.webp)

我们就以之前使用的 `randi_range` 函数为例

![图像](./Images/gds基础学习_6.webp)

## 数组

和其他语言不同 gdscript 可以支持数组中混合类型

```gd
var items = ["Potion", 3, 6]
```

当然我们也可以像创建变量一样规定数组的类型，就比如我们需要创建一个纯字符串的数组。

```gd
var items: Array[String] = ["Potion", "Feather", "Stolen harp"]
```

数组的访问访问顺序基本是所有语言通用的，从0开始数尝试打印出第一个元素和第三个

```gd
func _ready():
	var items: Array[String] = ["Potion", "Feather", "Stolen harp"]
	
	print(items[0])
	print(items[2])
```

我们也可以修改数组中的元素和添加删除数组中的元素。

```gd
extends Node

func _ready():
	var items: Array[String] = ["Potion", "Feather", "Stolen harp"]
	
	items[1] = "Smelly sock"
	items[2] = "Staff"
	
	items.remove_at(1)
	items.append("Overpowered sword")
```

## 循环

循环允许我们重复代码多次，适合遍历数组中的所有元素。

## for 循环

我们可以使用for循环来遍历数组中的所有元素。

```gd
extends Node

func _ready():
	var items: Array[String] = ["Potion", "Feather", "Stolen harp"]
	
	for item in items:
		print(item)
```

又或者只打印出数组中长度大于6个字符的元素'

```gd
extends Node

func _ready():
	var items: Array[String] = ["Potion", "Feather", "Stolen harp"]
	
	for item in items:
		if item.length() > 6:
			print(item)
```

我们还能创建运行特定次数的循环

```
for n in 8:
	print(n)
```

## while 循环

```gd
extends Node

func _ready():
	var glass := 0.0
	
	while glass < 0.5:
		glass += randf_range(0.01, 0.2)
		print(glass)
	print("杯子已经装满了一半")
```

如果我们想要提前结束循环就可以使用 `break` 或 `continue`，当我们glass大于0.2我们的循环就会提前结束。

```gd
extends Node

func _ready():
	var glass := 0.0
	
	while glass < 0.5:
		glass += randf_range(0.01, 0.2)
		print(glass)
		
		if glass > 0.2:
			break
		
	print("杯子已经装满了一半")
```

## 字典

```gd
extends Node

func _ready():
	#创建字典
	var players = {
		"Crook":1,
		"Villain":35,
		"Boss":100
	}
	#打印字典中的元素
	print(players["Villain"])

```

我们也可以修改或添加字典中的元素

```gd
extends Node

func _ready():
	#创建字典
	var players = {
		"Crook":1,
		"Villain":35,
		"Boss":100
	}
	
	# 修改字典中的元素
	players["Villain"] = 50
	
	# 添加元素
	players["Dwayne"] = 999
	
	# 遍历字典
	for username in players:
		print(username + ": " + str(players[username]))
```

输出结果：
```
Crook: 1
Villain: 50
Boss: 100
Dwayne: 999
```

我们甚至还能嵌套使用字典
```gd
extends Node

func _ready():
	var players = {
		"Crook": {"Level": 1, "Health": 80},
		"Villain": {"Level": 1, "Health": 80},
		"Boss": {"Level": 1, "Health": 80},
	}
	
	print(players["Boss"]["Health"])
```

## 枚举

假设我们要为游戏创建阵营分别为 `ALLY` `NEUTRAL` `ENEMY` ，然后再用if判断是否受到欢迎，如果是敌人就不欢迎，友军和中立一律为欢迎

```gd
extends Node

enum Alignment {ALLY, NEUTRAL, ENEMY}

var unit_alignment = Alignment.ALLY

func _ready():
	if unit_alignment == Alignment.ENEMY:
		print("你不受欢迎")
	else:
		print("欢迎")
```

`unit_alignment` 变量目前还是属于gd语言中写死的状态，我们可以使用之前学到的 `@export` 设置成检查器可见。

```gd
@export var unit_alignment = Alignment.ALLY
```
或
```gd
@export var unit_alignment: Alignment
```

然后我们就能直接在引擎的检查器中选择选项了

![图像](./Images/gds基础学习_7.webp)

假如我们尝试直接打印输出枚举中的一个类型

```gd
extends Node

enum Alignment {ALLY, NEUTRAL, ENEMY}

func _ready():
	print(Alignment.ENEMY)
```

最终输出会发现输出的是数字2，默认和数组一样是按照创建的顺序排列。

```
2
```

当然我们也可以手动给他赋值

```gd
extends Node

enum Alignment {ALLY = 1, NEUTRAL = -1, ENEMY = 0}

func _ready():
	print(Alignment.NEUTRAL)
```

## Match

Match语句类似于c++中的switch语句

```gd
extends Node

enum Alignment {ALLY, NEUTRAL, ENEMY}

@export var my_alignment: Alignment

func _ready():
	match my_alignment:
		Alignment.ALLY:
			print("Hello, friend!")
		Alignment.NEUTRAL:
			print("I come in piece")
		Alignment.ENEMY:
			print("TASTE MY WRATH!")
		_: #当以上情况都不符合就执行这里
			print("Who art thou?")
```

## 修改节点 2.0

当我们拖动节点到代码编辑器内的时候会产生一段相对路径

![图像](./Images/gds基础学习_8.webp)

当然我们也可以将这个引用存储在一个变量中，操作方法和上面差不多同样是拖拽过去，但是释放的时候要按住 <kbd>Ctrl</kbd> 再释放；godot这样就会帮助我们直接创建一个变量。

![图像](./Images/gds基础学习_9.webp)

`@onready` 只是确保godot等待所有子节点都创建完毕再执行，这样我们就不会遇到任何问题。

`$` 美元符号实际上是 `get_node` 函数的简写，实际上换成`get_node` 也是可以的。

```gd
@onready var weapon = get_node("Player/Weapon")
```

### 相对路径

现在我们的路径使用的是相对路径，因为我们的脚本是添加再 `Main` 节点上的所以，他是从 `Main` 的下一层开始找。

![图像](./Images/gds基础学习_10.webp)

### 绝对路径

我们也可以使用 `get_path()` 来获得我们的绝对路径

```gd
extends Node

@onready var weapon = get_node("Player/Weapon")

func _ready():
	print(weapon.get_path())
```

控制台输出

```
/root/Main/Player/Weapon
```

## 更灵活

以上的方法可能还不够灵活一但修改节点名称代码就会失效，而且最好只在我们想要访问的节点是当前工作节点的子节点时才使用路径。

所以我们可以使用 `@export` 来引用节点

```gd
extends Node

@export var my_node: Node

func _ready():
	pass
```

然后就能在godot的检查器中选择我们的节点了

![图像](./Images/gds基础学习_11.webp)

### 检查节点

我们可以用if来判断我们选择的节点是否为 `Node2D` 节点类型

```gd
extends Node

@export var my_node: Node

func _ready():
	if my_node is Node2D:
		print("Is 2D!")
```

### 规定节点类型

假设我们想让节点像变量的静态一样规定数据类型，也就是规定特定节点的类型那么我们可以把代码写成如下

```gd
@export var my_node: Sprite2D
```

现在我们只能分配精灵节点而无法选择其他类型的节点

![图像](./Images/gds基础学习_12.webp)

## 信号

选中按钮切换到节点我们可以看到，右侧有很多自带的信号

![图像](./Images/gds基础学习_13.webp)

我们双击 `perssed()` 信号

![图像](./Images/gds基础学习_14.webp)

点击连接

![图像](./Images/gds基础学习_15.webp)

然后我们在代码中就能看到 `func _on_button_pressed():` 函数，点击左侧绿色的图标可以看到我们信号的来源等信息。

![图像](./Images/gds基础学习_16.webp)

我们在这个按钮的函数下打印些什么

```gds
extends Node

func _ready():
	pass

func _on_button_pressed():
	print("按钮被按下了")
```

然后运行程序点击窗口中的按钮就会输出 "按钮被按下了" 的提示。

![图像](./Images/gds基础学习_17.webp)


我们添加一个 Timer 节点，并在检查器中把 Autostart 打开，然后添加Timer节点的 timeout信号。
并填入如下代码

```gd
extends Node

var xp := 0

func _on_timer_timeout():
	xp += 5
	print(xp)
	if xp >= 20:
		xp = 0
```

现在我们可以用 `signal` 创建一个其他节点可以连接的信号，我们可以把自己创建的这个leveled_up的信号给连接到其他节点，但是因为是演示所以我们就把他连接到main节点就好了。

```gd
extends Node

signal leveled_up

var xp := 0

func _on_timer_timeout():
	xp += 5
	print(xp)
	if xp >= 20:
		xp = 0
```

我们可以通过 `.emit` 发出信号

```gd
extends Node

signal leveled_up

var xp := 0

func _on_timer_timeout():
	xp += 5
	print(xp)
	if xp >= 20:
		xp = 0
		# 发出 leveled_up 信号
		leveled_up.emit()

# 连接的leveled_up信号
func _on_leveled_up():
	print("DING")
```

输出结果

```
5
10
15
20
DING
5
10
15
```

接下来我们要通过代码连接信号，所以第一步我们要在编辑器中断开 `_on_leveled_up` 的信号

```gd
signal leveled_up

var xp := 0

func _ready():
	# 连接信号
	leveled_up.connect(_on_leveled_up) # 注意函数名称后面没有括号

func _on_timer_timeout():
	xp += 5
	print(xp)
	if xp >= 20:
		xp = 0
		# 发出 leveled_up 信号
		leveled_up.emit()

# 连接的leveled_up信号
func _on_leveled_up():
	print("DING")
```

输出结果

```
5
10
15
20
DING
5
10
15
```

然后我们还能让信号传递一些比如等级或其他有用信息并打印它，比如这里我们就添加一个名为 `msg` 的变量来传递一些其他信息。

```gd
extends Node

signal leveled_up(msg) # <-- 添加一个名为msg的变量用来传递信息

var xp := 0

func _ready():
	# 连接信号
	leveled_up.connect(_on_leveled_up)

func _on_timer_timeout():
	xp += 5
	print(xp)
	if xp >= 20:
		xp = 0
		# 发出 leveled_up 信号
		leveled_up.emit("GZ!") # <-- 这里相当于给msg赋值

# 连接的leveled_up信号
func _on_leveled_up(msg): # <-- 这里也填入msg
	print(msg) # <-- 这里也填入msg
```

输出结果

```
5
10
15
20
GZ!
```

## Get/Set

get 和 set 使我们能够在变量改变时候添加代码，这意味着我们可以做比如把一个值限制在某个范围内。
或者发出一个信号，让代码的其他部分知道变量已更改。

### Set

对于此用途经常使用的例子是生命周值。

```gd
extends Node

signal health_changed(new_health) # <-- 创建信号

var health:= 100:
	set(value):
		health = clamp(value, 0, 100) # <-- 限制变量最小和最大范围
		health_changed.emit(health) # <-- 发出信号
		
func _ready():
	health = -150 # <-- 我们输入的生命值

func _on_health_changed(new_health):
	print(new_health) # <-- 最后打印出被限制转换后的生命
```

### Get

Get 则更常用于转换值

```gd
extends Node

var chance := 0.2

var chance_pct: int:
	get:
		return chance * 100
	set(value):
		chance = float(value) / 100.0

func _ready():
	print(chance_pct)
	chance_pct = 40
	print(chance_pct)
```

## 类

在写代码之前我们先创建一个 `Node` 节点，并命名为 `Character`，再为`Character`创建一个节点。

![图像](./Images/gds基础学习_18.webp)

我们的类就写成如下

```gd
class_name Character

extends Node

@export var profession: String # 设置角色职业
@export var health: int # 设置角色生命值

func die():
	health = 0
	print(profession + " 死了")
```

然后再我的的main.gd的脚本中写入

```gd
extends Node

@export var character_to_kill: Character # 选择角色

func _ready():
	character_to_kill.die() # 杀死当前角色
```

接下来我们就可以从Main节点的检查器中选择一个我们要杀死的角色

![图像](./Images/gds基础学习_19.webp)

比如我选择的是骑士，那控制台提示的就是

```
骑士 死了
```

## 内部类

