## 压缩转换
简单压缩转换（视频，图片都有效） <br>
`ffmpeg -i input.mkv output.mp4`

序列帧转视频 <br>
`ffmpeg -f image2 -i ./tmp/%04d.png movie.mp4`
+ image2 表示输入或输出文件的格式是image2格式
+ -f 是 format 格式的意思

## 修改分辨率

如果是宽高都缩放到原始图片的一半，则可以是乘以 0.5 或 除以 2，像下面这样写：

```shell
ffmpeg -i input.jpg -vf "scale=iw*.5:ih*.5" input_half_size.png
ffmpeg -i input.jpg -vf "scale=iw/2:ih/2" input_half_size.png
```
## 剪辑
简单裁剪视频<br>
`ffmpeg -i input.mp4 -ss 01:36 -to 01:45 output.mp4 `
+ -i 文件，input.mp4 为待处理源文件,output.mp4;
+ -ss 裁剪时间，后跟裁剪开始时间，-to后面是裁剪到裁剪结束时间;

`ffmpeg -i input.mp3 -ss 00:00:xx -t 120 output.mp3`
+ -ss 裁剪时间，后跟裁剪开始时间，以及 -t 裁剪时间；

## 提取音轨

~~~shell
ffmpeg -i input.mp4 -vn -acodec copy output.aac
~~~

其中，-i 表示输入文件，input.mp4 是您要提取音轨的视频文件名。-vn 表示不包含视频流，只包含音频流。-acodec copy 表示直接复制音频流，不进行重新编码。output.aac 是输出文件名。

## 合并音轨

~~~shell
ffmpeg -i input.mp4 -i input.aac -c:v copy -c:a copy output.mp4
~~~

其中，-i 表示输入文件，input.mp4 是您要添加音轨的视频文件名，input.aac 是您要添加的音轨文件名。-c:v copy 表示直接复制视频流，不进行重新编码。-c:a copy 表示直接复制音频流，不进行重新编码。output.mp4 是输出文件名。

## 压缩码率

```shell
ffmpeg -i Desktop/1.mov -b:v 1.5M  Desktop/1.mp4
```

- -b:v 1.5M : 指定码率
- -b:v :指定视频的码率
- -b:a : 指定音频的码率
- 1.5M：码率的值 1.5M 表示 1.5Mb/s