# <center>git使用指南</center>

## <b>Git基本设置</b>

设置名称和邮箱  

```shell
git config --global user.name "名称"
git config --global user.email "邮箱"
```

查看名称和邮箱

```shell
git config user.name "名称"
git config user.email "邮箱"
```

添加一个远程仓库地址链接  

``` shell
git remote add origin https://地址
```


## <b>Git基本命令</b>

  <b>克隆</b>   
   ` git clone 仓库地址 `  

  <b>克隆指定分支</b>   
  ` git clone -b 分支名称 仓库地址 `   
   >-b：这是branch的简写，代表分支的意思；

## <b>拉取 推送 查看</b>
查看远程仓库  
`git remote -v`

初始化仓库  
`git init`

在暂存区单独添加某个文件    
` git add test.txt `

将添加目录下所有文件加入暂存区  
` git add . `

查看暂存区的文件  
` git ls-files ` 

删除暂存区文件  
` git rm --cached <file> `

将暂存区提交到本地仓库  
`git commit`  
` git commit -m "提交内容" `

查看日志  
` git log `

查看上次提交之后是否有对文件进行再次修改  
`git status`

+ `git status -s`以精简的方式显示文件状态  
+ 
新添加的未跟踪文件前面有 `??` 标记  

	M = 修改 > 文件的内容或者mode被修改了.
	A = 已添加 > 你本地新增的文件（服务器上文件没有新增）
	D = 已删除 > 本地删除的文件（服务器上文件还在）.
	R = 重命名 > 文件名被修改
	C = 复制 > 文件的一个拷贝
	U = 已更新但尚未装入 > 文件没有被合并(需要完成合并才能进行提交)
	T = 文件的类型被修改了 > 文件的类型被修改
	
原文链接：https://blog.csdn.net/qq_44785877/article/details/113769747

<b>推送至网盘</b>  
` git push -u origin main ` 推送到指定分支 | main代表主分支名称  

` git push ` 推送到当前分支

` git pull `拉取

> 如果拉取遇到错误，可以先将当前的内容存储起来，`git stash`就可以把当前内容存储在栈内
> `git stash list` 可以查看临时存储栈内的列表

## <b>分支</b>

↓ 创建分支  
```shell
# 创建新分支
git branch [分支名称]

# 创建新分支并立即切换过来,一步到位.
git checkout -b [分支名称]

# 从某次提交中创建分支
git branch [分支名称] [commit]
git checkout -b [分支名称] [commit]
# 从某次提交中创建分支-具体用法
git branch dev1 7f65b72a08caa8d1b7969e1b385bea5b43f924ac
git checkout -b dev1 7f65b72a08caa8d1b7969e1b385bea5b43f924ac
```

查看分支  
` git branch ` → 本地分支  
`git branch -r` → 远程仓库分支  
` git branch -a` → 所有分支  

创建分支并设置为主分支  
` git branch -M [分支名称] `

切换分支  
` git checkout [分支名称] `


## 合并分支  
` git merge [分支名称] `

` git merge --no-ff [分支名称] `（推荐使用）

`--no-ff` 在这的作用是禁止快进式合并；从合并后的代码来看，结果其实是一样的，区别就在于 `--no-ff` 会让 Git 生成一个新的提交对象。为什么要这样？通常我们把 master 作为主分支，上面存放的都是比较稳定的代码，提交频率也很低，而 feature 是用来开发特性的，上面会存在许多零碎的提交，快进式合并会把 feature 的提交历史混入到 master 中，搅乱 master 的提交历史。所以如果你根本不在意提交历史，也不爱管 master 干不干净，那么 `--no-ff` 其实没什么用。不过，如果某一次 master 出现了问题，你需要回退到上个版本的时候，比如上例，你就会发现退一个版本到了 B，而不是想要的 F，因为 feature 的历史合并进了 master 里。

~~~
// git merge

          A---B---C feature
         /         master
D---E---F 

// git merge --no-ff

          A---B---C feature
         /         \
D---E---F-----------G master
~~~


###  ❌解决合并冲突
**二选一方法**
+ 保留本地代码：`git checkout --ours fileName`
+ 保留合并分支代码： `git checkout --theirs fileName`

相当于，在发生冲突的时候，–ours和–theirs会选择保留策略，执行完之后，git add 即可，不会再有任何冲突。

## 拉取远程分支到本地
1. **方法一：git checkout targetbranch**
~~~shell
1）首先，获取远程所有分支
    git fetch 
 
2）查看所有远程分支，找到需要的远程分支，例如 origin/targetbranch
    git branch -r 
 
3）在本地新建一个同名分支，然后系统会自动与该远程分支关联
    git checkout targetbranch
~~~

2. **方法二：git checkout -b 本地分支名 origin/远程分支名**
~~~shell
1）首先，获取远程所有分支
    git fetch 
 
2）创建与远程分支关联的本地分支（可以同名，也可以不同名；建议同名，方便管理）
    git checkout -b 本地分支名 origin/远程分支名
~~~

3. **方法四：git checkout -t origin/远程分支名**
~~~shell
1）首先，获取远程所有分支
    git fetch 
2）创建与远程分支关联的本地分支
    git checkout -t origin/远程分支名
~~~
4. **方法三：git checkout --track origin/远程分支名**
~~~shell
1）首先，获取远程所有分支
    git fetch 
 
2）创建与远程分支关联的本地分支
    git checkout --track origin/远程分支名 
~~~
### ❌git远程分支删除后，本地git branch -a或-r还能看到被删除的分支的解决方法

删除远程仓库已经不存在的分支

````shell
git remote prune origin
````

## 删除分支
删除分支  
`git branch -d [分支名称] ` → 删除一个分支, -d选项只能删除已经参与过合并的分支，对于未参与合并的分支是无法删除的。如果想强制删除一个分支，可以使用 `-D` 选项  

`git push origin --delete [分支名称]` → 删除远程分支

## 撤销修改
`git reset`撤销之前所有的 `git add` 操作   

`git reset 文件名`撤销指定文件的 `git add` 操作  

`git reset --hard 91a90dbd831a31646c0f0eac897d470e81d53e7f`回退到某个暂存区版本  

## 放弃文件修改

↓ 此命令用来放弃掉所有还没有加入到缓存区（就是 git add 命令）的修改：内容修改与整个文件删除
```shell
git checkout .
```


↓ 此命令用来清除 git 对于文件修改的缓存。相当于撤销 git add 命令所在的工作。在使用本命令后，本地的修改并不会消失，而是回到了第一步1. 未使用git add 缓存代码，继续使用用`git checkout .`，就可以放弃本地修改。
```shell
git reset HEAD
```

## git stash 命令

就在此时，线上版本master出现了bug，我们应该放下手头上新功能的开发工作先将master上的bug修复，这个时候dev分支下的改动怎么处理？ - 向dev分支提交新功能的代码，然后再切换到master下 - 直接切换到master分支下

首先我们新功能的代码还没开发完成，其次新功能这里还有一些bug没解决，就这样把有问题的代码提交到dev分支中，虽然可以解决目前我们的处境但不是很妥；但是第二种方案，直接切换，明显更不妥。怎么办？我们好像陷入了困境……

Git提供了一个**git stash 命令**恰好可以完美解决该问题, 其将当前未提交的修改(即，工作区的修改和暂存区的修改)先暂时储藏起来，这样工作区干净了后，就可以切换切换到master分支下拉一个fix分支。在完成线上bug的修复工作后，重新切换到dev分支下通过**git stash pop**命令将之前储藏的修改取出来，继续进行新功能的开发工作

↓ 具体用法

    # 储藏
    git stash

    # 将之前储藏的修改取出来
    git stash pop


## .gitignore 文件
设置Git忽略的文件，这些文件不参与Git库的提交与管理。
```
#表示此为注释,将被Git忽略


*.a     #表示忽略所有 .a 结尾的文件   


/build/   #忽略build下所有的文件
```

## 设置代理

如下代码是设置代理的方案，但是IP地址需要看个人情况一般是 `本机IP:VPN端口号`。

```shell
git config --global http.proxy 'http://192.168.0.1:1080'
git config --global https.proxy 'http://192.168.0.1:1080'
## 取消
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 仓库瘦身
你可以使用以下命令删除所有提交历史记录，但保留代码的当前状态1:
~~~shell
git checkout --orphan latest_branch
git add -A
git commit -am "commit message"
git branch -D master
git branch -m master
git push -f origin master
~~~
这将创建一个新的分支，其中包含您的当前代码状态，然后将其推送到远程存储库。如果您想要删除所有提交历史记录并将其与新的远程存储库关联，可以使用以下命令1:
~~~shell
rm -rf .git
git init
git add .
git commit -m "Initial commit"
git remote add origin <remote repository URL>
git push -u --force origin master
~~~
请注意，这将删除所有提交历史记录，并且您将无法恢复它们。因此，请确保在执行此操作之前备份您的代码。

## 仓库地址变动
~~~shell
git remote rm origin  #删除关联的origin的远程库
git remote add origin <remote repository URL> #添加远程仓库地址
~~~
# 常见错误

### ❌`git add .` 出现错误
`git add .`的时候无反应提示如下↓ ，可能是你命令操作的位置不在项目根目录。
```shell
hint: Use -f if you really want to add them.
hint: Turn this message off by running
hint: "git config advice.addIgnoredFile false"
```

### ❌`git pull`错误

`git pull` 拉取时候出现<font color="#dd0000">The following untracked working tree files would be overwritten by merge:</font><br/> 

```shell
git clean -n
// 是一次 clean 的演习, 告诉你哪些文件会被删除，不会真的删除
 
git clean -f
// 删除当前目录下所有没有 track 过的文件
// 不会删除 .gitignore 文件里面指定的文件夹和文件, 不管这些文件有没有被 track 过
 
git clean -f <path>
// 删除指定路径下的没有被 track 过的文件
 
git clean -df
 
// 删除当前目录下没有被 track 过的文件和文件夹
 
git clean -xf
 
// 删除当前目录下所有没有 track 过的文件.
// 不管是否是 .gitignore 文件里面指定的文件夹和文件
 
git clean 
// 对于刚编译过的项目也非常有用
// 如, 他能轻易删除掉编译后生成的 .o 和 .exe 等文件`在这里插入代码片`. 这个在打包要发布一个 release 的时候非常有用
 
git reset --hard
git clean -df
git status
// 运行后, 工作目录和缓存区回到最近一次 commit 时候一摸一样的状态。
// 此时建议运行 git status，会告诉你这是一个干净的工作目录, 又是一个新的开始了！
```

### ❌GitExtensions.exe: No such file or directory
当git commit的时候出现 GitExtensions.exe: No such file or directory等提示我们可以把编辑器修改回vim

`git config --global core.editor 'vim'`

### ❌路径错误
假如我们在解决冲突文件二选一的时候出现
error: pathspec 'Assets/ArtMrg/Model/Membrane' did not match any file(s) known to git
error: pathspec 'packs/金属0.mat' did not match any file(s) known to git
> 错误：路径规范“Assets/ArtMrg/Model/MMembrane”与git已知的任何文件都不匹配
> 错误：pathspec包/金属0.mat'与git已知的任何文件都不匹配

这是我们实际的文件路径：Assets/ArtMrg/Model/Membrane packs/金属0.mat

我们发现上方的文件路径中 Membrane packs这个文件夹中有一个空格，git并识别不了这个空格我们就需要改成：Membrane" "packs 这样；或者用两个单引号''把整个路径给圈起来如

方法1 在空格位置增加双引号
~~~ bash
Assets/ArtMrg/Model/Membrane" "packs/金属0.mat
~~~
方法2 把整个路径使用单引号围起来
~~~ bash
`Assets/ArtMrg/Model/Membrane packs/金属0.mat`
~~~

### ❌.gitignore不生效，不忽略
决方法: git清除本地缓存（改变成未track状态），然后再提交:  
~~~ shell
git rm -r --cached .  
git add .  
git commit -m ‘update .gitignore’  
git push -u origin master
~~~


core.autocrlf是一个Git变量，用于处理行结束符。它可以设置为三个值：true、input和false。如果你在Windows上使用Git，可以将其设置为true，以便在签出代码时将LF转换为CRLF。如果你在Mac或Linux上使用Git，则可以将其设置为input，以便在签出代码时不进行自动转换。你可以使用以下命令来设置core.autocrlf：git config --global core.autocrlf true。
`git config --global core.autocrlf true`

# <center>仓库创建</center>

## 从命令行创建一个新的仓库
~~~shell
touch README.md
git init
git checkout -b main
git add README.md
git commit -m "first commit"
git remote add origin http://192.168.0.114:3000/JAZZ/ChenGuang.git
git push -u origin main
~~~

## 从命令行推送已经创建的仓库
~~~shell
git remote add origin http://192.168.0.114:3000/JAZZ/ChenGuang.git
git push -u origin main
~~~

# 打包为zip和tar

~~~ shell
// 打包master分支的所有文件
git archive --format=zip --output master.zip master
~~~

# Git reset和git revert的区别

+ git reset 是回滚到对应的commit-id，相当于是删除了commit-id以后的所有的提交，并且不会产生新的commit-id记录，如果要推送到远程服务器的话，需要强制推送-f

+ git revert 是反做撤销其中的commit-id，然后重新生成一个commit-id。本身不会对其他的提交commit-id产生影响，如果要推送到远程服务器的话，就是普通的操作git push就好了

# 中文路径乱码或被数字代替

~~~ shell
git config --global core.quotepath false
~~~

这个命令是用来设置Git的全局配置，使得Git在处理含有非ASCII字符的路径时不进行URL编码。core.quotePath选项默认值为true，表示对非ASCII字符进行编码。设置为false则表示不进行编码。

在某些操作系统或者终端环境中，如果文件名包含非ASCII字符或特殊字符，Git可能会自动对这些字符进行URL编码，这可能会导致问题。例如，在Windows系统中，如果文件名包含空格或引号，这些字符在Git中可能被错误地处理。设置core.quotePath为false可以避免这个问题。
