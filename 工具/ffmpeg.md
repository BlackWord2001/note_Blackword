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