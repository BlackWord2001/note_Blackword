## 压缩转换
简单压缩转换（视频，图片都有效） <br>
`ffmpeg -i input.mkv output.mp4`

序列帧转视频 <br>
`ffmpeg -f image2 -i ./tmp/%04d.png movie.mp4`
+ image2 表示输入或输出文件的格式是image2格式
+ -f 是 format 格式的意思

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