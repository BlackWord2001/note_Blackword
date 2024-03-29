# Linux目录

[基础指令](#基础指令)

[Vim](#vim)

[screen](#screen)


<a id = "基础指令"> </a>

## 基础指令
|指令|作用|
|:-|:-
|`mkdir`|创建文件夹
|`tree`|查看当前文件目录的树形图
|`cp`|复制文件
|`realpath`|查看文件路径
|`pwd`|显示当前目录完整路径
|`rm`|删除；`-r`删除目录；`-f`强制删除
|`mv`|移动文件
|`shutdown -h now`|现在关机
### 解压缩文件
+ 压缩指定文件夹 <br>
`tar -zcvf filename.tar.gz dir filename`

<a id = "Vim"> </a>

## Vim
|指令|作用
|:-|:--
|`:q`|退出
|`:q!`|强制退出
|`:w`|保存
|`:qw`|保存并退出
|`:wq!`|强制保存并退出
|`:e!`|放弃所有修改，从上次保存文件开始再编辑
|`:/`|搜索，按<kbd>N</kbd>寻找下一个

|快捷键|作用
|:-|:--
|<kbd>V</kbd>|多选
|<kbd>C</kbd>|剪切
|<kbd>P</kbd>|粘贴


<a id = "screen"> </a>

## **screen**
Linux screen命令用于多重视窗管理程序。
screen为多重视窗管理程序。此处所谓的视窗，是指一个全屏幕的文字模式画面。通常只有在使用telnet登入主机或是使用老式的终端机时，才有可能用到screen程序。

```
-A 将所有的视窗都调整为目前终端机的大小。
-d <作业名称> 　将指定的screen作业离线。
-h <行数> 　指定视窗的缓冲区行数。
-m 即使目前已在作业中的screen作业，仍强制建立新的screen作业。
-r <作业名称> 　恢复离线的screen作业。
-R 先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
-s <shell> 　指定建立新视窗时，所要执行的shell。
-S <作业名称> 　指定screen作业的名称。
-v 显示版本信息。
-x 恢复之前离线的screen作业。
-ls或--list 　显示目前所有的screen作业。
-wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。
```