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

// 关闭lfs
git lfs uninstall
~~~


Git LFS clone 命令已经被弃用，因为 Git clone 命令已经更新，速度与 Git LFS clone 命令相当。因此，现在 Git LFS clone 命令与 Git clone 命令几乎相同，而单独的命令将消失

已经弃用的命令 ↓
~~~shell
git lfs clone

git lfs pull
~~~

## 删除本地 Git LFS 文件

摘自：https://zhuanlan.zhihu.com/p/146683392
官方手册：https://github.com/git-lfs/git-lfs/blob/6340befc60876f4f039f215479d9d5a945f817e1/docs/man/git-lfs-prune.adoc#L4

你可以使用 git lfs prune 命令从本地 Git LFS 缓存中删除文件：

```shell
$ git lfs prune
✔ 4 local objects, 33 retained
Pruning 4 files, (2.1 MB)
✔ Deleted 4 files
```

这将删除所有被认为是旧的本地 Git LFS 文件。 旧文件是以下未被引用的任何文件：

+ 当前检出的提交
+ 尚未推送（到 origin，或任何 lfs.pruneremotetocheck 设置的）的提交
+ 最近一次提交

默认情况下，最近的提交是最近十天内创建的任何提交。 通过添加以下内容计算得出：

+ 获取额外的 Git LFS 历史记录中讨论的 lfs.fetchrecentrefsdays 属性的值（默认为 7）； 至
+ lfs.pruneoffsetdays 属性的值（默认为 3）

![iamge](./Images/git_lfs_1.png)

你可以配置 prune 偏移量以将 Git LFS 内容保留更长的时间：

```shell
# don't prune commits younger than four weeks (7 + 21)
$ git config lfs.pruneoffsetdays 21
```

与 Git 的内置垃圾收集不同，Git LFS 内容不会自动修剪，因此，定期运行 git lfs prune 命令是保持本地仓库大小减小的好主意。

你可以使用 git lfs prune --dry-run 来测试修剪操作将产生什么效果：

```shell
$ git lfs prune --dry-run
✔ 4 local objects, 33 retained
4 files would be pruned (2.1 MB)
```

以及使用 git lfs prune --verbose --dry-run 命令精确查看哪些 Git LFS 对象将被修剪：

```shell
$ git lfs prune --dry-run --verbose
✔ 4 local objects, 33 retained
4 files would be pruned (2.1 MB)
* 4a3a36141cdcbe2a17f7bcf1a161d3394cf435ac386d1bff70bd4dad6cd96c48 (2.0 MB)
* 67ad640e562b99219111ed8941cb56a275ef8d43e67a3dac0027b4acd5de4a3e (6.3 KB)
* 6f506528dbf04a97e84d90cc45840f4a8100389f570b67ac206ba802c5cb798f (1.7 MB)
* a1d7f7cdd6dba7307b2bac2bcfa0973244688361a48d2cebe3f3bc30babcf1ab (615.7 KB)
```

## .gitattributes
`.gitattributes` 中 `-text` 就是表示这个文件不是文本文件。其余的就是告诉 Git 在处理 filter、diff、merge 时将 pbtxt 文件通过 LFS 的方式处理，打开 .gitconfig 可以看到相关命令的替换。

# LFS 错误解决

##  error: external filter 'git-lfs filter-process' failed

大致报错

```
Use git lfs logs last to view the log.
error: external filter 'git-lfs filter-process' failed
fatal: data/processed/career_builder/embedding.npy: smudge filter lfs failed
warning: Clone succeeded, but checkout failed.
```

解决方法

```
// Skip smudge - We'll download binary files later in a faster batch
// 跳过污迹-我们稍后将以更快的批量下载二进制文件
git lfs install --skip-smudge

// Do git clone here
// 在此处执行git克隆
git clone ...

// Fetch all the binary files in the new clone
// 获取新克隆中的所有二进制文件
git lfs pull

// Reinstate smudge
// 恢复污迹
git lfs install --force
```