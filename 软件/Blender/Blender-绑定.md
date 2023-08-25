# <center>高级绑定课程笔记</center>

## 形变（Deform）

关闭形变选项可以确保blender不会给我们创建多余的定点组

## 骨骼对称
Naming Conventions 命名习惯，在Blender命名习惯不仅仅是用于找到正确的骨骼，也告诉Blender哪两个骨骼是配对骨骼。

如果你的骨架可以镜像另一半（即它是两侧对称），这是值得坚持左/右命名的约定。这将使您能够使用一些工具，可能会节省您的时间和精力（如 X轴镜像 的编辑工具，我们上面看到的...）。

![形态健](./images/animation_armatures_bones_editing_naming_example.png)

首先，你应该给你的骨骼有意义的基名称，如leg (腿部)，arm (手臂)，finger (手指)，back (背部)，foot (脚部)等。

如果你的骨骼的副本在另一边，像手臂，给它如下面的分隔符区别：

左/右分隔符可以是第二个位置 "L_calfbone"，也可以是最后一个位置 "calfbone.R"。

如果有一个大写或小写 L, R, left 或 right, Blender 正确处理副本。有关分隔符的列表，请参见下文。选择一个，当在绑定时尽量使用它们。会得到收获的。

    (nothing): handLeft --> handRight

    "_" (underscore): hand_L --> hand_R

    "." (dot): hand.l --> hand.r

    "-" (dash): hand-l --> hand-r

    " "（空格): 手向左 --> 手向右


## 形态键
> 形态键用于制作动画将物体变形为新形态。 在其他术语中，形态键可能被称为 "形变目标" 或 "混合形态" 。
形态键最常用的用例是用在角色面部动画，以及用在调整和改进骨架绑定。 它们对于模拟有机柔软部位和肌肉建模特别有用，这里需要对结果形态进行更多控制，而这些控制不是旋转和缩放组合可以实现的。

ShapeKeys-1 | ShapeKeys-2
:-: | :-:
![形态健](./images/ShapeKeys-1.png) | ![形态健](./images/ShapeKeys-2.png)

`特殊形态健` > `混合后的新形态` 从当前状态的形态健顶点中创建一个新的形态健，相当于把当前状态另存为一个新的形态健。 


