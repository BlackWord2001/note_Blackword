# OpenGL

## API

## 自建类
### shader类的使用方法
```glsl
Shader our_shader("path/to/shaders/shader.vs", "path/to/shaders/shader.fs");
...
while(...)
{
    our_shader.use();
    our_shader.set_float("someUniform", 1.0f);
    DrawStuff();
}
```

## 其他

OpenGL 4.5 在 OpenGL 3.3 的基础上增加了很多新特性。例如：

    多级纹理，允许将多个纹理绑定在一起，从而使用更多的纹理数据进行图形渲染。

    静态着色器，允许在编译时预先编译着色器，从而加快渲染速度。

    动态着色器，允许在运行时动态修改着色器，从而使着色器能够更灵活地适应不同的渲染需求。

    增强的 Tessellation 功能，允许对图形进行更精细的细分，从而提高渲染质量。

    新增的视口多段投影功能，允许使用多个视口进行投影，从而提高渲染效率。

    增强的半精度浮点运算能力，使得 OpenGL 4.5 可以使用半精度浮点数进行更复杂的图形渲染。

    增强的顶点格式支持，使得 OpenGL 4.5 可以更灵活地处理不同类型的顶点数据。

    这些只是 OpenGl 4.5 的部分新特性，实际上还有很多其他新功能，例如对调试和优化的增强支持等。