## **校正物体原点轴向与位置** <br>
+ 打开原点模式 (Origins)
+ 选中物体进入编辑模式 (EditMode)
+ 选中模型的其中一个面
+ 在3D视图上方坐标轴中点击`+`创建坐标系
+ 物体(Object)->变换(Transfrom)->对齐到变换坐标系(Align to Transfrom Orientation)
+ 设置原点(SetOrigins) -> 原点到几何中心 (Orifins to Geomtery)

## uv实时展开

在 `属性` 视图中的 `活动工具与工作区设置` 中有个 `UV 实时展开` 的选项，勾选后只要标记缝合边blender就会为你自动展开操作。

## 缝合边镜像
1. 选择缝合边 <kbd>Shift</kbd> + <kbd>G</kbd>
2. 选择→选择镜像 <kbd>Shift</kbd> + <kbd>Ctrl</kbd> + <kbd>M</kbd>

## 将骨骼设置为物体的父级

当我们需要给角色手上放某些物品比如武器等就需要将物体直接作为手部骨骼的子级。

1. 先在物体模式下，选择部件，然后再选骨骼。
2. 然后进入姿态模式，具体选中对应的骨骼。
3. <kbd>ctrl</kbd> - <kbd>p</kbd>  选择 `骨骼`，这样就绑定成功了。

    ![image](./images/bone-8.png)