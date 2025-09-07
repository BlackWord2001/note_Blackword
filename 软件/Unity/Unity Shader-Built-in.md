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

### 旋转UV
```hlsl
// 旋转uv函数
float2 RotateUV(float2 uv, float rotation)
{
    float rad = rotation * UNITY_PI / 180.0;
    float s = sin(rad);
    float c = cos(rad);
    float2 center = float2(0.5, 0.5);
    uv -= center;
    uv = float2(uv.x * c - uv.y * s, uv.x * s + uv.y * c);
    uv += center;
    return uv;
}
```

使用方法：在片表面着色器中创建一个`mainUV`用来接受`RotateUV`函数的`uv`返回值

```hlsl
void surf(Input IN, inout SurfaceOutputStandard o)
{
    // 把旋转后的UV用单独的一个变量存储
    float2 mainUV = IN.uv_MainTex;
    mainUV = RotateUV(mainUV, _UVRotation);

    // 最后把mainUV传递到需的地方
    fixed4 c = tex2D(_MainTex, mainUV) * _Color; 
    o.Albedo = c.rgb;
}
```

### 旋转UV - 法线
法线的uv旋转和一般普通color颜色旋转不一样，当UV旋转180度时，法线贴图的方向会反转，这是因为法线贴图存储的是切线空间中的方向向量。我们需要对法线本身进行反向旋转来补偿UV旋转带来的影响。

```hlsl
float3 RotateNormal(float3 normal, float rotation)
{
    // 1. 将角度转换为弧度
    float rad = rotation * UNITY_PI / 180.0;
    // 2. 计算正弦和余弦值
    float s = sin(rad);
    float c = cos(rad);
    
    // 创建旋转矩阵 (绕Z轴旋转)
    float3x3 rotMatrix = float3x3(
        c, -s, 0, // 第一行: X轴变换
        s, c, 0,  // 第二行: Y轴变换
        0, 0, 1   // 第三行: Z轴不变
    );
    // 4. 将法线与旋转矩阵相乘
    return mul(rotMatrix, normal);
}
```

表面着色器部分

```hlsl
void surf(Input IN, inout SurfaceOutputStandard o)
{
    //旋转后的uv传递
    float2 mainUV = IN.uv_MainTex;
    mainUV = RotateUV(mainUV, _UVRotation);

    // 采样并应用法线贴图
    half4 normalMap = tex2D(_BumpMap, mainUV);
    half3 normal = UnpackScaleNormal(normalMap, _BumpScale);
    
    // 对法线贴图进行旋转补偿 - 新增
    o.Normal = RotateNormal(normal, -_UVRotation);
}
```

**为什么绕Z轴旋转？**
+ 在切线空间中，法线向量的Z分量指向表面法线方向
+ X和Y分量表示切线方向
+ UV旋转是在2D平面上进行的，对应的是绕Z轴(法线方向)的旋转

**示例：180度旋转的情况**
当 _UVRotation = 180 时：

+ 弧度值: 180 × π/180 = π
+ sin(π) = 0, cos(π) = -1
+ 旋转矩阵变为:
```
[-1  0  0]
[ 0 -1  0]
[ 0  0  1]
```
这将使法线的X和Y分量取反，正好补偿了UV旋转180度带来的影响

### 视差高度纹理

```hlsl
// 陡峭视差映射函数（高质量）
float2 SteepParallaxMapping(Input IN, float2 rotatedUV, float rotation)
{
    // 旋转视角方向以匹配UV旋转
    float3 rotatedViewDir = RotateViewDir(IN.viewDir, rotation);
    
    // 计算每层深度
    float layerDepth = 1.0 / _ParallaxSteps;

    // 当前每层的深度
    float currentLayerDepth = 0.0;

    // 每层UV移动量
    float2 P = rotatedViewDir.xy * _ParallaxStrength;
    float2 deltaUV = P / _ParallaxSteps;
    
    //当前UV
    float2 currentUV = rotatedUV;

    // 从高度纹理采样当前深度
    float currentDepthMapValue = tex2D(_ParallaxMap, currentUV).r;

    //逐步追踪直到找到交点
    [unroll(16)] // 部分展开循环以提高性能
    for (int i = 0; i < _ParallaxSteps; i++)
    {
        if (currentLayerDepth > currentDepthMapValue)
            break;

        // 移动到下一层
        currentUV -= deltaUV;
        currentDepthMapValue = tex2Dlod(_ParallaxMap, float4(currentUV, 0, 0)).r;
        currentLayerDepth += layerDepth;
    }
    return currentUV;
}
```

其中的`RotateViewDir()`这个函数其实和之前的`RotateNormal()`旋转法线的函数是完全一样的，就只是换了个名称看起来更通用。

```hlsl
// 旋转视角方向
float3 RotateViewDir(float3 viewDir, float rotation)
{
    float rad = rotation * UNITY_PI / 180.0;
    float s = sin(rad);
    float c = cos(rad);
    
    float3x3 rotMatrix = float3x3(
        c, -s, 0,
        s, c, 0,
        0, 0, 1
    );
    
    return mul(rotMatrix, viewDir);
}
```