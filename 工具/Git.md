# git使用指南

## <b>Git基本设置</b>

设置名称和邮箱  
`git config --global user.name "名称"`  
`git config --global user.email 邮箱 `  
添加一个远程仓库地址链接  
` git remote add origin https://地址 `

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
新添加的未跟踪文件前面有 `??` 标记  
新添加到暂存区中的文件前面有 `A` 标记  
修改过的文件前面有 `M` 标记  


<b>推送至网盘</b>  
` git push -u origin main ` 推送到指定分支 | main代表主分支名称  

` git push ` 推送到当前分支

` git pull `拉取

> 如果拉取遇到错误，可以先将当前的内容存储起来，`git stash`就可以把当前内容存储在栈内
> `git stash list` 可以查看临时存储栈内的列表


## <b>分支</b>

创建分支  
` git branch [分支名称] `  
`git checkout -b [分支名称]` → 创建新分支并立即切换过来,一步到位.

查看分支  
` git branch ` → 本地分支  
`git branch -r` → 远程仓库分支  
` git branch -a` → 所有分支  

创建分支并设置为主分支  
` git branch -M [分支名称] `

切换分支  
` git checkout [分支名称] `

切换到主分支  
` git checkout master `

合并分支  
` git merge [分支名称] `

删除分支  
`git branch -d [分支名称] ` → 删除一个分支, -d选项只能删除已经参与过合并的分支，对于未参与合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项  

`git push origin --delete [分支名称]` → 删除远程分支

### 拉取远程分支到本地
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

## 撤销修改
`git reset`撤销之前所有的 `git add` 操作   

`git reset 文件名`撤销指定文件的 `git add` 操作  


`git reset --hard 91a90dbd831a31646c0f0eac897d470e81d53e7f`回退到某个暂存区版本  


## .gitignore 文件
设置Git忽略的文件，这些文件不参与Git库的提交与管理。
```
#表示此为注释,将被Git忽略


*.a     #表示忽略所有 .a 结尾的文件   


/build/   #忽略build下所有的文件
```

设置代理
```shell
# 设置http:
git config --global http.proxy http://127.0.0.1:1080
# 设置https:
git config --global https.proxy https://127.0.0.1:1080
# 设置socks:
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
## 取消
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 合并分支

合并操作建议先在master上先建立一个新的分支，如果要把其他分支合并到 master 主分支请先切换到master。  
`git merge 分支名`


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
