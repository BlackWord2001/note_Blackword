# 数学函数

 序号 | 算法 | ???
:---: | :--- | :---
1 | lerp | 插值算法


## lerp / mix (线性插值)

```hlsl
// 原理：在a和b之间线性插值
float result = lerp(a, b, t); // = a + (b - a) * t
```
作用：颜色混合、过渡效果、强度控制

## pow (幂函数)

```hlsl
// 原理：求x的y次幂
float result = pow(x, y);
```

作用：对比度调整、伽马校正、非线性衰减

## clamp (钳制/限制)

用于将数值限制在指定的最小值和最大值范围内

```hlsl
color = clamp(color, 0.0, 1.0);
```

作用：

防止数值溢出：颜色值超出0-1范围会导致不可预测的渲染结果

硬件限制：很多显示设备和格式期望颜色在0-1范围内

数学安全：避免后续计算中出现无效的数学操作

## dot （点积 Dot Product）

对于两个向量 a 和 b：
```hlsl
dot(a, b)
```

相当于

```hlsl
a.x * b.x + a.y * b.y + a.z * b.z
```

## max

定义：max(a, b) 返回两个值中较大的那个。

工作原理：
```hlsl
// 标量版本
float max(float a, float b)
{
    return (a > b) ? a : b;
}

// 向量版本（逐分量比较）
float3 max(float3 a, float3 b)
{
    return float3(
        (a.x > b.x) ? a.x : b.x,
        (a.y > b.y) ? a.y : b.y,
        (a.z > b.z) ? a.z : b.z
    );
}
```
示列：
```hlsl
float result1 = max(3.0, 5.0);    // 5.0
float result2 = max(-2.0, 0.0);   // 0.0

float2 vec1 = float2(1.0, 3.0);
float2 vec2 = float2(2.0, 1.0);
float2 result3 = max(vec1, vec2); // (2.0, 3.0)
```


## min

定义：min(a, b) 返回两个值中较小的那个。

工作原理：

```hlsl
// 标量版本
float min(float a, float b)
{
    return (a < b) ? a : b;
}
```

示列：

```hlsl
float result1 = min(3.0, 5.0);    // 3.0
float result2 = min(-2.0, 0.0);   // -2.0

float2 vec1 = float2(1.0, 3.0);
float2 vec2 = float2(2.0, 1.0);
float2 result3 = min(vec1, vec2); // (1.0, 1.0)
```


---

# 一些快捷操作

## Swizzle操作

你可能会在某些代码中看到 `.xxx` 等方式

```hlsl
float luminance = 0.6;        // 单个浮点数值
float3 gray = luminance.xxx;  // 等同于 float3(0.6, 0.6, 0.6)
```

这相当于

```hlsl
float3 gray = float3(luminance, luminance, luminance);
```

`luminance` 是单个浮点数值（标量）

`lerp` 函数需要两个相同维度的向量进行插值

`luminance.xxx` 将标量转换为三维向量，便于与 color 进行插值