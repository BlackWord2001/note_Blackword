## LUT

LUT 本质上是一个 颜色映射表，它将输入颜色替换为输出颜色。在 Godot 中，通常使用一张 256×16 像素 的特殊渐变图片。

Godot 支持的 LUT 图片通常是：

+ PNG 格式，带或不带透明度

+ 尺寸：通常为 256×16 像素（16×16 的色块，共 16 行）

+ 颜色空间：sRGB

![image](./Images/environment_adjustments_3d_lut_template.webp)

> LUT文件不同引擎直接可能并不通用标准也不太一样，推荐使用不同引擎从官方渠道重新下载官方提供的LUT文件。

