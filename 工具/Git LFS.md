# <center>Git LFS介绍</center>
LFS 的指针文件是一个文本文件，存储在 Git 仓库中，对应大文件的内容存储在 LFS 服务器里，而不是 Git 仓库中，下面为一个图片 LFS 文件的指针文件内容：

~~~
version https://git-lfs.github.com/spec/v1
oid sha256:5b62e134d2478ae0bbded57f6be8f048d8d916cb876f0656a8a6d1363716d999
size 285
~~~

Git LFS 是无缝的：在你的工作副本中，你只会看到实际的文件内容。这意味着你不需要更改现有的 Git 工作流程就可以使用 Git LFS。你只需按常规进行 git checkout、编辑文件、git add 和 git commit。git clone 和 git pull 将明显更快，因为你只下载实际检出的提交所引用的大文件版本，而不是曾经存在过的文件的每一个版本。

# 使用方法
1. 一旦安装好了 Git LFS，请运行 git lfs install 来初始化，新的仓库也需要使用此命令
~~~shell
git lfs install
~~~

2. LFS跟踪文件
~~~shell
// 跟踪文件
git lfs track '*.jpg'

// 查看已跟踪的文件
git lfs track

git lfs track '*.jpg'
~~~


Git LFS clone 命令已经被弃用，因为 Git clone 命令已经更新，速度与 Git LFS clone 命令相当。因此，现在 Git LFS clone 命令与 Git clone 命令几乎相同，而单独的命令将消失

已经弃用的命令 ↓
~~~shell
git lfs clone

git lfs pull
~~~

## .gitattributes
`.gitattributes` 中 `-text` 就是表示这个文件不是文本文件。其余的就是告诉 Git 在处理 filter、diff、merge 时将 pbtxt 文件通过 LFS 的方式处理，打开 .gitconfig 可以看到相关命令的替换。