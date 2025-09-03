> ⚠️以下内容部分为ai生成不一定完全准确

### 使用顶点着色器

在shaderlab中使用顶点着色器需要在 #pragma 后面加上 `vertex:vert` 才能起效果

```hlsl
#pragma surface surf Standard fullforwardshadows vertex:vert
```

### 顶点着色器的基础框架

```hlsl
void vert(inout appdata_full v, out Input o)
{
    // 在这里初始化
    UNITY_INITIALIZE_OUTPUT(Input, o);

    // 可以在下面添加代码
}
```

为什么需要 `UNITY_INITIALIZE_OUTPUT()`

> 未初始化的风险：在 HLSL/Cg 中，如果你声明一个结构体变量但没有显式初始化其所有成员，这些变量的值是未定义的。它们可能是任何值（之前内存中的垃圾数据）。

> Shader 的不可预测性：这些未定义的值会导致你的着色器计算出现极其随机和不可预测的结果。你可能看到闪烁、奇怪的条纹、颜色错误，或者在不同显卡、不同平台上表现不一致。

> 并非所有字段都需要赋值：在你的自定义顶点函数 (vert) 中，你可能只修改了 Input 结构体中的一两个字段（比如 worldPos 或 customUV）。而其他你没有赋值的字段（比如传统的 uv_MainTex）如果保持未初始化状态，就会被传递到 surf 函数中，导致基于它们的纹理采样等操作出错。

宏的内部实现，它将整个结构体 name 的所有内存区域用 0 填充

```hlsl
#define UNITY_INITIALIZE_OUTPUT(type,name) name = (type)0;
```

这提供了一个干净、一致的起点。之后你的代码只需要为你关心的字段赋值，其他字段会自动保持为 0，而不是垃圾值。

### 简单的波浪
```hlsl
// 简单的波浪效果测试  
float wave = sin(_Time.y * 5 + v.vertex.x * 10) * _WaveAmount;  
v.vertex.y += wave;
```