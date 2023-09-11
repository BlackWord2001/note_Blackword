# <center> 高级绑定 </center>

高级绑定课程对blender基础有一定要求，建议能熟练使用blender后再看高级绑定课程，否测很多操作可能会看不懂。

## 01 Deformation skeleton

__骨骼名称中英文对照表__

__English__ | __中文__
:--- | :---
ROOT| 根
HEAD | 头部
NECK | 脖子
CHEST | 胸部 
SPINE | 脊柱 
HIP | 臀部 
LEG | 腿
SHIN | 小腿
FOOT | 脚
TOE | 脚趾
SHOULDER | 肩膀
ARM | 臂 
FORARM | 前臂 
HAND | 手
FINGER | 手指
THUMB | 拇指

给角色增加最基本的骨骼，命名以“DEF”开头。以及手臂和腿的部分需要对称镜像的骨骼在名称最后加上`.L` 的后缀名，这样在（编辑模式）下全选骨骼，右键选择（对称）就能识别并创建出 `.R` 后缀的镜像骨骼。



↓ 绑定参考图

![图像](./Images/高级绑定P01-2.png)

↓ 脚骨骼侧面图

![图像](./Images/高级绑定P01-3.png)

↓ 手指

![图像](./Images/高级绑定P01-4.png)

↓ ROOT骨骼

> 绑定时一定要注意骨骼坐标轴的朝向，Z轴永远朝上或者朝前。
![图像](./Images/高级绑定P01-5.png)


↓ DEF骨骼最后完成的样子


正视图 | 侧视图
:---: | :---:
![图像](./Images/高级绑定P01-1.png) | ![图像](./Images/高级绑定P01-6.png)

## 02 骨骼管理器

接下来就要使用（Bone Manager）插件对骨骼进行分类管理，

插件下载地址请查看：[Blender-插件.md](../Blender-插件.md)

这部分我们要做的很简单，只需要专门新建一个ROOT层，并选中ROOT骨骼点击右侧的圆圈“○”给放进去就ok了。

![图像](./Images/高级绑定P01-7.png)

## 03 Assigning armature

因为绑定的模型可能会导入游戏引擎所以TGT骨骼是专门用来专门控制DEF骨骼的，相当于是一层控制器。

1. 第一步需要我们在（编辑模式）下把除了ROOT骨骼将DEF开头的骨骼全都选中按 <Kbd>Shift</Kbd> + <Kbd>D</Kbd> 复制一份。

2. 把复制出来的骨骼全部放到一个新的层级中，并把层级命名为TGT。

    ![图像](./Images/高级绑定P01-8.png)

3. 隐藏其他层就单独显示TGT骨骼层，然后将TGT层的骨骼全选，按下<Kbd>Ctrl</Kbd> + <Kbd>F2</Kbd> 进行批量重命名把DEF的前缀替换成TGT。

    ![图像](./Images/高级绑定P01-9.png)

4. 清除blender给我们创建的后缀，复制后的物体因为和原物体重名blender会给我们在名称末尾加上.001 .002 ...等编号，现在我们需要删除他。保险起见我们分两步操作

    > 先删除末尾的数字
    ![图像](./Images/高级绑定P01-10.png)

    > 再删除标点
    ![图像](./Images/高级绑定P01-11.png)

5. 然后就是把DEF捆绑在TGT上，这里我们使用约束修改器，先选择DEF再选中相应的TGT骨骼，按下<Kbd>Ctrl</Kbd> + <Kbd>Shift</Kbd> + <Kbd>C</Kbd> 选择（Copy Transforms）复制变换就好了，接下来就是体力活把所有DEF骨骼都绑到对应的TGT骨骼上。

    ![图像](./Images/高级绑定P01-12.png)

    ↓ 添加完（Copy Transforms）约束后就可以在DEF的约束面板看见它

    ![图像](./Images/高级绑定P01-13.png)

6. 在刷权重之前我们需要把ROOT和TGT开头的所有骨骼的（形变）全部关闭，要不然blender会给我们创建这些骨骼的定点组，我们只需要DEF骨骼驱动人物模型就可以了，因此我们并不需要这些顶点组。

    ![图像](./Images/高级绑定P01-14.png)

7. 先选中物体再加选骨骼按下 <Kbd>Ctrl</Kbd> + <Kbd>P</Kbd> 选择（附带空顶点组），就完成物体和骨骼的关联了
   
    ![图像](./Images/高级绑定P01-15.png)

    ↓ 顶点组的名称会和骨骼名称一模一样，简单的模型如机器人等关节分离的模型可以直接在这个面板中使用（指定）按钮创建权重。

    ![图像](./Images/高级绑定P01-16.png)

    （编辑模式）下打开顶点组权重选项，再选择对应的顶点组就能看到直观的权重影响范围。

    ![图像](./Images/高级绑定P01-17.png)

8. 刷权重部分省略....
   
## 04 躯干控制器

把DEF层先给隐藏，只留下TGT和ROOT方便观察

![图像](./Images/高级绑定-18.png)

从（TGT-HIPS）骨骼沿着Y轴挤出一条名为（CTRL-TORSO） 

![图像](./Images/高级绑定-19.png)

新建一个层把他命名为（TORSO）将（CTRL-TORSO）放到这个新建的层中

![图像](./Images/高级绑定-20.png)

复制一根（TGT-HIPS）骨骼并命名为（CTRL-HIPS）用来作为臀部控制，然后选中这根骨骼并使用快捷键 <kbd>Alt</kbd> - <kbd>F</kbd> 来反转他，并把将（CTRL-HIPS）使用设置为 （CTRL-TORSO）的子级。

![图像](./Images/高级绑定-21.png)

再拉出一根名为（CTRL-CHEST）的骨骼用来控制我们胸部或躯干整个上半部分，也设置为CTRL-TORSO的子集。

![图像](./Images/高级绑定-22.png)

把其他层的骨骼都给隐藏只留下TORSO层最后效果如下

![图像](./Images/高级绑定-23.png)

↓ 骨骼父子关系参考如下

![图像](./Images/高级绑定-24.png)

基础的控制骨骼已经创建完毕，接下来就开始实现控制躯干和臀部的功能

1. 先把 _TGT_HIPS_ 的父级设置为 _CTRL_HIPS_

    ![图像](./Images/高级绑定-25.png)

    > 但是现在出现了一个问题，当我们去旋转 _CTRL_HIPS_ 骨骼整个角色都会旋转，这根骨骼的作用是单纯用来控制臀部的接下来将修复这个问题。

    以上问题根本的原因还是因为 _TGT_SPINE_ 的父级是 _TGT_HIPS_ 当这根骨骼旋转的时候同时也会带动 _TGT_SPINE_ 所以会出现臀部以上也跟着一起旋转的效果。解决办法也很简单只需要把 _TGT_SPINE_ 的父级设置为 _CTRL_TORSO_ 就可以了。

    ![图像](./Images/高级绑定-26.png)

2. 先选中 _CTRL-CHEST_ 再加选 _TGT-SPINE_ ，<kbd>Ctrl</kbd> - <kbd>Shift</kbd> - <kbd>C</kbd> → [ __Copy Rotation__ ](https://docs.blender.org/manual/zh-hans/3.6/animation/constraints/transform/copy_rotation.html) 

    - 目标和拥有者都设置为（局部空间）
    - Mix设置为（相加），或者用（偏移）也可以

    以上步骤设置完后就可以使用 _CTRL_SPINE_ 控制 _TGT_SPINE_ 了，_TGT_SPINE_ 也可以单独进行旋转。

    ![图像](./Images/高级绑定-27.png)

    如果没有把Mix设置为（相加）_TGT_SPINE_ 就无法单独旋转。

    ![图像](./Images/高级绑定-28.png)

3. 先选中 _CTRL-CHEST_ 再加选 _TGT-CHEST_ ，<kbd>Ctrl</kbd> - <kbd>Shift</kbd> - <kbd>C</kbd> → [ __Copy Rotation__ ](https://docs.blender.org/manual/zh-hans/3.6/animation/constraints/transform/copy_rotation.html)

    ![图像](./Images/高级绑定-29.png)

    最终效果就是只需要旋转 _CTRL_SPINE_ 就能同时控制 _TGT-SPINE_ 和 _TGT-CHEST_

    ![图像](./Images/高级绑定-30.png)

4. 最后一步我们要来修改骨骼的名称，因为 _TGT_CHEST_ 和 TGT_SPINE 可以直接允许控制腰部胸部的旋转所以我们也给他改名并归类为 CTRL。
   
    - _TGT_CHEST_ → _CTRL_TWEAKS_CHEST_
    - _TGT_SPINE_ → _CTRL_TWEAKS_SPINE_

    以上两根骨骼名称修改完后，再将其分类进 _TORSO_ 骨骼层

    ![图像](./Images/高级绑定-31.png)

## 05 头部和颈部

