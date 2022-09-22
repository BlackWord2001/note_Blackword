## <b>配置Git</b>

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
` git push -u origin main `
> main代表主分支名称  

` git push `

拉取  
` git pull `
> 如果拉取遇到错误，可以先将当前的内容存储起来，`git stash`就可以把当前内容存储在栈内
> `git stash list` 可以查看临时存储栈内的列表


## <b>分支</b>

创建分支  
` git branch 分支名称 `  

查看所有分支  
` git branch `

创建分支并设置为主分支  
` git branch -M 分支名称 `

切换分支  
` git checkout 分支名称 `

切换到主分支  
` git checkout master `

合并分支  
` git merge 分支名称 `

## 撤销修改
  撤销之前所有的 `git add` 操作   
`git reset`

撤销指定文件的 `git add` 操作  
`git reset 文件名`

回退到某个暂存区版本  
`git reset --hard 91a90dbd831a31646c0f0eac897d470e81d53e7f`

## .gitignore 文件
设置Git忽略的文件，这些文件不参与Git库的提交与管理。
```
#表示此为注释,将被Git忽略


*.a：     #表示忽略所有 .a 结尾的文件   


/build/   #忽略build下所有的文件
```