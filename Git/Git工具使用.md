# 配置Git

+ <b>设置名称和邮箱</b>  
`git config --global user.name "名称"`  
`git config --global user.email 邮箱 `

# Git基本命令
+ <b>克隆</b>   
  + ` git clone 仓库地址 `  

+ <b>克隆指定分支</b>   
  + ` git clone -b 分支名称 仓库地址 `   
   >-b：这是branch的简写，代表分支的意思；


# 添加上传

在暂存区单独添加某个文件  
` git add test.txt `

将添加目录下所有文件加入暂存区  
` git add . `

提交  
`git commit`