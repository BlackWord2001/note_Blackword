# ohmyposh 配置

## windows

### 主题安装
编辑启动文件

```shell
code $PROFILE
```

设置主题如下：
```shell
oh-my-posh init pwsh --config 'D:/oh-my-posh-Themes/froczh.omp.json' | Invoke-Expression
```
但是出现如下报错

```shell
. : 无法加载文件 C:\Users\black\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1，因为在此系统上禁止运行脚
本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 3
+ . 'C:\Users\black\Documents\WindowsPowerShell\Microsoft.PowerShell_pr ...
+   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

解决方法

```shell
set-executionpolicy remotesigned
```

### 字体安装

Oh My Posh 旨在使用 Nerd 字体 。书字体是流行的字体，经过修补以包含图标。 我们建议使用 Meslo LGM NF， 但任何书字体都应该与标准 主题 兼容。 

Meslo 下载地址： https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/Meslo.zip

Nerd 字体大全：https://www.nerdfonts.com/