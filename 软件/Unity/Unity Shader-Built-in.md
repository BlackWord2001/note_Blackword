> ⚠️以下内容部分为ai生成不一定完全准确
## 基础方法

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

### 双面渲染

```hlsl
//始终启用双面渲染
Cull Off

CGPROGRAM
```

但双面渲染会导致法线出现问题所以我们要对代码做出一些修改

不再手动干预法线方向，让Unity处理双面渲染的法线计算

可以增加一个 `_BackfaceIntensity` 参数控制背面光照强度

```hlsl
Properties
{
    ...
    
    // 增加 BackfaceIntensity 参数控制背面光照强度
    [Header(Double Sided Lighting)]
    _BackfaceIntensity("Backface Light Intensity", Range(0,1)) = 0.5
}

SubShader
{

    Cull Off //始终启用双面渲染

    CGPROGRAM
    
    struct Input
    {
        ...
        
        float faceSign : VFACE; // 使用VFACE语义自动检测正面和背面
    };

    half _BackfaceIntensity; // _BackfaceIntensity声明

    ...

    void surf(Input IN, inout SurfaceOutputStandard o)
    {
        ...

        // 采样并应用法线贴图
        half4 normalMap = tex2D(_BumpMap, IN.uv_MainTex);
        o.Normal = UnpackScaleNormal(normalMap, _BumpScale);
        
        // 使用Unity内置的VFACE功能处理双面光照
        // IN.faceSign将为正面返回1，背面返回-1
        if (IN.faceSign < 0)
        {
            // 调整背面光照强度
            o.Albedo *= _BackfaceIntensity;
        }
    }

}
```

当然双面渲染还能制作成控制开关unity中有个这样的内置方法可以使用

```hlsl
Properties
    {
        [Header(Rendering Settings)]
        // Off = 双面显示 (0), Front = 剔除正面 (1), Back = 剔除背面 (2)
        [Enum(UnityEngine.Rendering.CullMode)] _Cull ("Cull Mode", Float) = 0

        ...
    }

    SubShader
    {
        // 关闭剔除
        Cull [_Cull]

        ...
    }
```

这样在材质面板中就会出现一个下拉选框，可以选择 `正面剔除` `背面剔除` `不剔除` 三种模式

## 实际案列

### 条纹边界
这个Shader的核心思想是利用世界空间坐标来生成一个跨越整个3D空间的连续条纹图案，并用它来混合两种颜色。这种方法确保了条纹在模型表面、甚至在不同模型的接缝处都是连续的。

**1.概述 (Summary)**
特性 | 说明
:---: | :---
Shader 名称 | Blackword-Shader/Blackword_Boundary 
效果|在模型表面生成具有移动、密度的3D 连续条纹，通常用于表现物体边界或特殊视觉效果。
渲染队列 | Transparent（透明）
关键技术 | 利用世界坐标 (pos.x + pos.y + pos.z)  计算 3D 条纹，确保跨面连续性。

**2.Properties（属性）**
属性名 | 类型 | 描述 |默认值
:---: | :---: | :--- | :---
_BaseColor | Color | 基础颜色 (背景色) | (0, 0.5, 1, 0.2)
_StripeColor | Color | 条纹颜色  | (0, 1, 1, 1)
_StripeDensity | Float | 条纹密度 (相当于 Tiling/平铺次数) | 5.0 
_StripeWidth | Range(0, 1) | 条纹宽度 (0-1 之间)  | 0.5 

**3.Rendering Settings (渲染设置)**

属性名 | 类型 | 描述 | 默认值
:---: | :---: | :--- | :---
_Cull | Enum | 剔除模式：Off (双面显示)、Front (剔除正面)、Back (剔除背面) | 0 (Off) 

**4.SubShader 核心设置**

设置项 | 值 | 目的/说明
:---: | :--- | :---
Tags| "Queue"="Transparent" "RenderType"="Transparent" "IgnoreProjector"="True" | 强制在透明物体队列中渲染，并且忽略投影机（Projector）的影响。
Blend | SrcAlpha OneMinusSrcAlpha | 开启标准的Alpha 混合（透明度混合）。
ZWrite | Off |关闭深度写入，这是渲染透明物体时的标准做法，防止错误地遮挡后面的物体。
Cull | [_Cull] | 根据用户设置的 _Cull 属性进行面片剔除。

**源码**

```hlsl
Shader "Blackword-Shader/Blackword_Boundary"
{
    Properties
    {
        [Header(Rendering Settings)]
        // Off = 双面显示 (0), Front = 剔除正面 (1), Back = 剔除背面 (2)
        [Enum(UnityEngine.Rendering.CullMode)] _Cull ("Cull Mode", Float) = 0
        
        [Header(Color)]
        _BaseColor ("Base Color (Background)", Color) = (0, 0.5, 1, 0.2)
        _StripeColor("Stripe Color", Color) = (0, 1, 1, 1)
        
        [Header(Stripe Settings)]
        _StripeDensity ("Density (Tiling)", Float) = 5.0
        _StripeWidth("Stripe Width (0-1)", Range(0, 1)) = 0.5
        
        [Header(Options)]
        _ScrollSpeed("Scroll Speed", Float) = 0.0
    }
    SubShader
    {
        // 关键设置：渲染队列设置为透明，忽略投影
        Tags { "Queue"="Transparent" "RenderType"="Transparent" "IgnoreProjector"="True"}
        LOD 100
        
        // 开启alpha混合
        Blend SrcAlpha OneMinusSrcAlpha
        
        // 关闭深度写入
        ZWrite Off
        
        // 关闭剔除
        Cull [_Cull]
        
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #pragma multi_compile_fog

            #include "UnityCG.cginc"

            // 输入顶点数据
            struct appdata
            {
                // 顶点位置
                float4 vertex : POSITION;
                // 顶点法线，用于计算世界法线
                float3 normal : NORMAL; 
            };

            // 顶点着色器到片元着色器传递的数据
            struct v2f
            {
                // 裁剪空间（屏幕）坐标
                float4 vertex : SV_POSITION;
                // 核心：传递世界坐标
                float3 worldPos : TEXCOORD0;
                // 核心：传递世界法线
                float3 worldNormal : TEXCOORD1;
                // 传递雾效坐标
                UNITY_FOG_COORDS(2)
            };
            
            float4 _BaseColor;
            float4 _StripeColor;
            float _StripeDensity;
            float _StripeWidth;
            float _ScrollSpeed;

            // 顶点着色器
            v2f vert (appdata v)
            {
                v2f o;
                // 将顶点位置从模型空间转换到裁剪空间
                o.vertex = UnityObjectToClipPos(v.vertex);
                
                // 将模型空间坐标转换到世界空间，并传递给片元着色器
                o.worldPos = mul(unity_ObjectToWorld, v.vertex).xyz;

                // 将模型空间法线转换到世界空间
                o.worldNormal = UnityObjectToWorldNormal(v.normal);
                
                UNITY_TRANSFER_FOG(o,o.vertex);
                return o;
            }

            // 条纹函数
            float GetStripePattern(float3 pos)
            {
                // 核心算法改为 (x + y + z)
                // 这相当于定义了一组法线方向为 (1,1,1) 的 3D 平面切割物体
                // 无论在哪个面，它们都是同一个数学函数计算出来的，因此在接缝处绝对连续
                // 乘以 _StripeDensity 控制疏密，加上 _Time.y * _ScrollSpeed 实现随时间滚动
                float val = (pos.x + pos.y + pos.z) * _StripeDensity + _Time.y * _ScrollSpeed;
                // frac 取小数部分形成循环，Step 根据宽度截断形成的硬边
                return step(_StripeWidth, frac(val));
            }

            fixed4 frag (v2f i) : SV_Target
            {
                // 计算条纹: 使用传入的 i.worldPos 计算条纹图案
                float stripePattern = GetStripePattern(i.worldPos);
                
                // 颜色混合: 使用 lerp (线性插值) 函数，根据 stripePattern 的值 (0 或 1) 来混合 _BaseColor 和 _StripeColor
                fixed4 col = lerp(_BaseColor, _StripeColor, stripePattern);
                
                // 应用雾效: 应用 Unity 内置的雾效
                UNITY_APPLY_FOG(i.fogCoord, col);
                
                // 返回最终颜色
                return col;
            }
            ENDCG
        }
    }
}
```