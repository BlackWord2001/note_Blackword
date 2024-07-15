# 语法

## 注释

### 文档注释
文档注释用于为代码中的成员提供文档说明，通常用于更详细的描述。文档注释以 ## 开头，并且必须写在成员声明之前或脚本顶部。例如：
```gd
## 这是一个文档注释
## 用于描述下面的变量
var my_variable = 10
```

文档注释也可以用于导出变量，在编辑器中通过悬停查看。例如：

```gd
## 这是一个导出变量的文档注释
@export var my_exported_variable = 20
```

# 运算符

## is and or

is 运算符：

用于检查一个变量是否是某个类的实例或其子类的实例。
例如：

```
if my_node is Node2D:
    print("my_node 是 Node2D 的实例")
```

or 运算符：

用于布尔逻辑运算，表示逻辑或操作。只要有一个条件为真，整个表达式就为真。
例如：

```
if condition1 or condition2:
    print("至少有一个条件为真")
```

and 运算符：

用于布尔逻辑运算，表示逻辑与操作。只有当所有条件都为真时，整个表达式才为真。
例如：

```
if condition1 and condition2:
    print("两个条件都为真")

```



# 函数

## func _unhandled_input(event):

在 Godot 中，func _unhandled_input(event): 是一个用于处理未被消耗的输入事件的函数。这个函数在输入事件没有被 _input() 或任何 GUI 元素处理时被调用。输入事件会沿着节点树向上传递，直到某个节点处理它为止。

## _input(event):

在 Godot 中，_input(event) 函数用于处理所有输入事件。与 _unhandled_input(event) 不同，_input(event) 会在输入事件传递到节点树的每个节点时被调用。你可以在这个函数中处理各种输入事件，例如键盘、鼠标和触摸事件。

## clamp()

● Variant clamp(value: Variant, min: Variant, max: Variant)

钳制 value，返回不小于 min 且不大于 max 的 Variant。任何能够用小于和大于运算符进行比较的值都能工作。

var a = clamp(-10, -1, 5)
a 是 -1

var b = clamp(8.1, 0.9, 5.5)
b 是 5.5
