 + ## <b>配置Git</b>

设置名称和邮箱  
`git config --global user.name "名称"`  
`git config --global user.email 邮箱 `  
添加一个远程仓库地址链接  
` git remote add origin https://地址 `

 + ## <b>Git基本命令</b>

  <b>克隆</b>   
   ` git clone 仓库地址 `  

  <b>克隆指定分支</b>   
  ` git clone -b 分支名称 仓库地址 `   
   >-b：这是branch的简写，代表分支的意思；

+ ## <b>添加上传</b>

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

<b>推送至网盘</b>  
` git push -u origin main `
> main代表主分支名称  

` git push `


+ ## <b>分支</b>

创建分支  
` git branch 分支名称 `  

创建分支并设置为主分支
` git branch -M 分支名称 `

切换分支  
` git checkout 分支名称 `

切换到主分支  
` git checkout master `

合并分支  
` git merge 分支名称 `