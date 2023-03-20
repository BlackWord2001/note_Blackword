# <center>UGUI</center>

+ Canvas（画布）：对象上依附的 ↓
	+ Canvas：画布组件，主要用于渲染UI控件
	+ Canvas Scaler：画布分辨率自适应组件，主要用于分辨率自适应
		+ UI 缩放模式（建议修改为屏幕大小缩放），参考分辨率设置为1920x1080
	+ Graphic Raycaster：射线事件交互组件，主要用于控制射线响应相关
	+ RectTransform：UI对象位置锚点控制组件，主要用于控制位置和对其方式
+ EventSystem对象上依附的 ↓
	+ EventSystem和Standalone Input Module: ;
	+ 玩家输入事件响应系统和独立输入模块组件，主要用于监听玩家操作

> Canvas的意思是画布它是UGUI中所有U|元素能够被显示的根本它主要负责渲染自己的所有U子对象；如果Ul控件对象不是Canvas的子对象,那么控件将不能被渲染我们可以通过修改Canvas组件上的参数修改渲染方式。

> 场景中允许有多个Canvas对象可以分别管理不同画布的渲染方式，分辨率适应方式等等参数；如果没有特殊需求一般情况场景 上一个Canvas即可。

三种渲染方式 | 
:-: | 
![image](./images/10.jpg) | 




+ Canvas的三种渲染方式 ↓
	1. Screen Space - Overlay：屏幕空间，覆盖模式，Ul始终在前
	2. Screen Space - Camera：屏幕空间，摄像机模式，3D物体可以显示在UI之前
	3. World Space：世界空间，3D模式

+ Pixel Perfect（像素完美）：是否开启无锯齿精确渲染(性能换效果)
+ SortOrder（排序次序）：排序层编号(用于控制多个Canvas时的渲染先后顺序)
+ TargetDisplay（目标显示）：设置在哪个显示设备上显示
+ Additional Shader Channels（附加着色器通道）：其他着色器通道，决定着色器可以读取哪些数据

## <center>Screen Space - Camera</center>
渲染摄像机：不推荐使用主摄像机使用，最好新建一个附摄像机单独渲染UI内容
> 注： 相机的`图层`要设置成UI，否则会把UI以外比如场景也一起渲染出来

## <center>Text 文本</center>
自动调整文本框大小：`添加组件` → `Layout` → `Content Size Fitter` 把水平匹配和垂直适配都修改成 `Preferred Size`。

字体材质的三个值  
1. Face（基本设置）
	+ Color：字体颜色
	+ Softnees：字体模糊强度
	+ Dilate：字体扩张
2. Outline（轮廓）
	+ Color：字体轮廓描边颜色
	+ Thickness：字体外圈大小
1. Underlay（背影）
	+ Color：背影颜色
	+ Offset X：X轴偏移
	+ Offset Y：Y轴偏移
	+ Dilate：背景颜色强度
	+ Softness：模糊