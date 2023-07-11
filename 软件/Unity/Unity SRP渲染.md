# <center>渲染</center>

## Volume
+ Exposure（曝光控制）

+ Visual Envronment（环境控制）

+ Fog（雾效）

+ Lighting （光照）

## <center>静态光照</center>
### 室内光
不推荐使用材质自发光，自发光每个灯光都是靠材质球的发射强度控制的，需要细微调整的时候也不是那么方便，目前建议使用在平面光去代替那些自发光的材质球进行渲染，效果调整更方便。

### 生成照明
1. 当间接光烘焙出现噪点和黑色斑块可以通过修改 **降噪器设置** 推荐使用`OpenImageDenoise` 降噪器, **间接过滤器** 设置为`Gaussian`半径设置为最大值5；
2. 推荐打开 `环境光遮蔽`，看模型大小一般最大距离 `1` 是完全够用的，直接贡献可以调整AO阴影的颜色深度，我一般推荐调淡一些 比如：`0.25`，直接贡献强度太高阴影颜色会很深显得非常不真实。

### ⚠:自发光无法烘焙
+ 如果自发光烘焙无效果需要把 `Exposure weight` 的值调为0。
+ 检查 **全局光照** 选项中是否选择的是`Realtime`，如果渲染没效果大概选择的是 `无（None）`。
+ **全局光照** 这个选项需要保证所有需要烘焙光照的网格必须开着 `Realtime` ，要不然会烘焙不上。

![image](./images/1.jpg)

## <center>反射探针</center>
反射探针选择 `盒体` 在室内效果会更立体更真实

## <center>屏幕空间反射</center>

HDRP管线默认是不开启屏幕空间反射的需要手动开启，URP没有内置SSR。

在 `项目设置` → `质量` → `HDRP` 选择我们目前正在使用的配置（HDRP Hogj Fidelity），然后再选择 `光照` → `反射` 中的屏幕空间反射以及下方的透明的也都打开；这时候我们就能在 Volume 中加入屏幕空间反射

![image](./images/11.jpg)

# <center>材质</center>
## 贴花
### 关闭个别材质的贴花投影
选择材质设置中的 `Receive Decals` 关闭它；
![image](./images/12.jpg)

## MaskMap
MaskMap：遮罩贴图是多种贴图的混合，它包含四个灰度纹理，每个纹理有一个颜色通道。

通道 | 纹理
:- | :-
R | Metallic 贴图 （金属）  
G | Occlusion 贴图 （遮蔽）  
B | Detail mask 贴图（细节）  
A | Smoothness 贴图 （光滑）

![image](./images/maskmap_0.png)

### 使用Krita合成MaskMap

1. 先把 Metallic，Occlusion，Smoothness 全部导入进krita的图层中，DetailMask我们现在暂时不会用到所以直接在krita新建一个图层命名为Detail，然后填充为白色。

2. 以上导入的图像属于灰度图所以在krita暂时无法使用RGBA通道，先将导入的图层全选，`菜单栏` ->`图像` -> `转换图像色彩空间` 将 `色彩模型` 这个选项选择为`RGB/透明度`后确认。

![image](./images/maskmap_1.jpg)

3. 然后我们再右键图层 `属性`单独设置活动通道，然后导出就可以了。

![image](./images/maskmap_2.jpg)

![image](./images/maskmap_3.jpg)

## 阴影距离
在通用渲染管线 (URP) 中，请在通用渲染管线资源中设置 Shadow Distance 属性。

在高清渲染管线 (HDRP) 中，请为每个体积 (Volume) 设置 Shadow Distance 属性。