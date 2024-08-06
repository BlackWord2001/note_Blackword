## ❌:DNS

如果上传有问题先试试看自己设置DNS

dns推荐：https://zhuanlan.zhihu.com/p/339495904

可以先ping一下想用的dns看看延迟等情况。

## ❌:Attempted to load the data for an avatar we do not own, clearing blueprint ID

这个问题的解决方法来自这个视频：https://www.bilibili.com/video/BV1Gr421c7mq

视频介绍：一行代码（完全）解决上传问题（包括地图和模型上传问题），原因在于上传封面时会调用官网域名。

那就换个能用的（分析闲聊在下文）


大概在第1075行加入：

```cs
uploadUrl = uploadUrl.Replace("vrchat.com", "api.vrchat.cloud");
```

请根据上下代码插入（我那改过可能会挫个几行，不同版本SDK也大致在那个位置）

-最近模型上传不了，于是顺着源码摸过去，发现在上传的API里会分类上传，通过插入Debug发现上传封面时云端会下发配置文件重定向调用官网域名。
（官网在6月2日进不去了，DNS被污染SNI也糊了，不过只影响官网，游戏是另一个域名不影响。

继续经测试发现，，捏嘛嘛滴，官网传完图片后还是用的API域名，那为何不直接调用API传，套娃是吧【
所以，代码的作用仅仅是识别和替换API，相当于fix了个BUG吧。


1. 从错误中打开这个文件
    ![images](./Images/上传错误-1.webp)

2. 找到大概这个位置添加代码
    ![images](./Images/上传错误-2.webp)

3. 重新上传问题解决！