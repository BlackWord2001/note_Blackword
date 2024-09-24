# BodyPaint和ps协作

BodyPaint3D原生支持psd文件作为贴图的存储格式而不是交换格式，所以使用bodypaint必定会用到ps。

我们可以在BodyPaint3D设置中，把默认格式改为psd格式，这样新建材质球文件的时候默认就选择的是psd格式而不是tif

![图像](./images/BP3D和PS联合设置-1.webp)

## ps不更新psd文件

当我们在BP3D中绘制了内容然后保存这时候psd文件已经更新，但是已经从ps打开的ps却没有从硬盘中加载新的内容。

这时候我们只需要

PS ：编辑-首选项-常规-自动更新打开文件 （打钩） 这样 BP保存的贴图文件 就可以及时更新到PS里

![图像](./images/BP3D和PS联合设置-2.webp)

## bp3d更新ps

bp这边也同理但是需要手动更新比较麻烦点。

BP：在PS修改后  到BP 贴图下 右键-纹理-恢复纹理  然后就可以更新PS修改过的贴图了 

![图像](./images/BP3D和PS联合设置-3.webp)

或者材质设置中点重载图像也可以

![图像](./images/BP3D和PS联合设置-4.webp)