# learnopengl 学习笔记

OpenGL 4.5 在 OpenGL 3.3 的基础上增加了很多新特性。例如：

    多级纹理，允许将多个纹理绑定在一起，从而使用更多的纹理数据进行图形渲染。

    静态着色器，允许在编译时预先编译着色器，从而加快渲染速度。

    动态着色器，允许在运行时动态修改着色器，从而使着色器能够更灵活地适应不同的渲染需求。

    增强的 Tessellation 功能，允许对图形进行更精细的细分，从而提高渲染质量。

    新增的视口多段投影功能，允许使用多个视口进行投影，从而提高渲染效率。

    增强的半精度浮点运算能力，使得 OpenGL 4.5 可以使用半精度浮点数进行更复杂的图形渲染。

    增强的顶点格式支持，使得 OpenGL 4.5 可以更灵活地处理不同类型的顶点数据。

    这些只是 OpenGl 4.5 的部分新特性，实际上还有很多其他新功能，例如对调试和优化的增强支持等。


# shader.h
```glsl
Shader ourShader("path/to/shaders/shader.vs", "path/to/shaders/shader.fs");
...
while(...)
{
    ourShader.use();
    ourShader.setFloat("someUniform", 1.0f);
    DrawStuff();
}
```

## glDrawArrays , glDrawElements

`glDrawArrays` 和 `glDrawElements` 的作用都是从一个数据数组中提取数据渲染基本图元。

使用区别：

gl.glDrawArrays 设置顶点和纹理buffer的时候，位置已经写好了

gl.glDrawElements order是按照你设置的顶点和纹理的顺序

性能区别：

glDrawArrays 传输或指定的数据是最终的真实数据,在绘制时效能更好
glDrawElements 指定的是真实数据的调用索引,在内存/显存占用上更节省

glDrawArrays 和 glDrawElements的损耗说明及其使用场景

glDrawArrays 主要讲数据空间损耗在顶点的定义处;
glDrawElements 主要讲数据空间损耗在顶点索引的定义处;
如果在你的工程中，画的图形较少或者，图形虽多但很多相同的，则可采用glDrawArrays更节省数据占用的空间；相反，如果图形多，而且形状大不相同的时候，可以优先考虑采用glDrawElements函数。

glDrawArrays , glDrawElements 这两个方法的第一个形参都是代表 绘制模式(GLenum mode)。绘图模式有如下几种：

`GL_POINTS`， `GL_LINE_STRIP`， `GL_LINE_LOOP`， `GL_LINES`， `GL_LINE_STRIP_ADJACENCY`，`GL_LINES_ADJACENCY`， `GL_TRIANGLE_STRIP`，`GL_TRIANGLE_FAN`， `GL_TRIANGLES`，`GL_TRIANGLE_STRIP_ADJACENCY`，`GL_TRIANGLES_ADJACENCY` and `GL_PATCHES`

其中 `GL_LINE_STRIP_ADJACENCY`，`GL_LINES_ADJACENCY`，`GL_TRIANGLE_STRIP_ADJACENCY`  和`GL_TRIANGLES_ADJACENCY`，只有3.2后才支持。

    GL_POINTS - 单独的将顶点画出来。

    GL_LINES - 单独地将直线画出来。行为和 GL_TRIANGLES 类似。

    GL_LINE_STRIP - 连贯地将直线画出来。行为和 GL_TRIANGLE_STRIP 类似。

    GL_LINE_LOOP - 连贯地将直线画出来。行为和 GL_LINE_STRIP 类似，但是会自动将最后一个顶点和第一个顶点通过直线连接起来。

    GL_TRIANGLES - 这个参数意味着OpenGL使用三个顶点来组成图形。所以，在开始的三个顶点，将用顶点1，顶点2，顶点3来组成一个三角形。完成后，在用下一组的三个顶点(顶点4，5，6)来组成三角形，直到数组结束。

    GL_TRIANGLE_STRIP - OpenGL的使用将最开始的两个顶点出发，然后遍历每个顶点，这些顶点将使用前2个顶点一起组成一个三角形。

![image](./images/1.png) | ![image](./images/2.png)
---|---

## Z缓冲

OpenGL存储它的所有深度信息于一个Z缓冲(Z-buffer)中，也被称为深度缓冲(Depth Buffer)。GLFW会自动为你生成这样一个缓冲（就像它也有一个颜色缓冲来存储输出图像的颜色）。深度值存储在每个片段里面（作为片段的z值），当片段想要输出它的颜色时，OpenGL会将它的深度值和z缓冲进行比较，如果当前的片段在其它片段之后，它将会被丢弃，否则将会覆盖。这个过程称为深度测试(Depth Testing)，它是由OpenGL自动完成的。

然而，如果我们想要确定OpenGL真的执行了深度测试，首先我们要告诉OpenGL我们想要启用深度测试；它默认是关闭的。我们可以通过`glEnable`函数来开启深度测试。`glEnable`和`glDisable`函数允许我们启用或禁用某个OpenGL功能。这个功能会一直保持启用/禁用状态，直到另一个调用来禁用/启用它。现在我们想启用深度测试，需要开启`GL_DEPTH_TEST：`

```c++
glEnable(GL_DEPTH_TEST);
...

// 主函数循环
while (!glfwWindowShouldClose(main_window))
{
    /* 因为我们使用了深度测试，我们也想要在每次渲染迭代之前清除深度缓冲（否则前一帧的深度信息仍然保存在缓冲中）。
    就像清除颜色缓冲一样，我们可以通过在glClear函数中指定DEPTH_BUFFER_BIT位来清除深度缓冲 */
    glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
}

```

### glUniformMatrix4fv
我们首先查询`uniform`变量的地址，然后用有`Matrix4fv`后缀的`glUniform`函数把矩阵数据发送给着色器，第二个参数告诉OpenGL我们将要发送多少个矩阵，第三个参数询问我们是否希望对我们的矩阵进行转置(Transpose)，也就是说交换我们矩阵的行和列。OpenGL开发者通常使用一种内部矩阵布局，叫做列主序(Column-major Ordering)布局。GLM的默认布局就是列主序，所以并不需要转置矩阵，我们填`GL_FALSE`。后一个参数是真正的矩阵数据，但是GLM并不是把它们的矩阵储存为OpenGL所希望接受的那种，因此我们要先用GLM的自带的函数`value_ptr`来变换这些数据。

~~~ c++
// 变换矩阵
glUniformMatrix4fv(modelLoc, 1, GL_FALSE, glm::value_ptr(model));
// 观察矩阵
glUniformMatrix4fv(view_loc, 1, GL_FALSE, &view[0][0]);
~~~
> 如果已经下载了 `shader.h` 这个头文件将不用手写 `glUniformMatrix4fv` 直接使用 `ourShader.("矩阵", 矩阵);`

# glm
**glm::perspective**
```
glm::mat4 proj = glm::perspective(glm::radians(45.0f), (float)width / (float)height, 0.1f, 100.0f);
```
第一个参数 `glm::radians(45.0f)` 代表视野(Field of View) 数值越大视野更宽。     
第二个参数设置了宽高比，由视口的宽除以高所得。  
第三和第四个参数设置了平截头体的近和远平面(最近最远渲染距离)。


