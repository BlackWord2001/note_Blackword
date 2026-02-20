# powershell 配置

## utf-8

> 编程环境最好统一使用utf8编码来减少终端输出字符出现乱码等问题

打开powershell配置文件路径
```bash
$PROFILE
```

可以使用vscode打开
```bash
code $PROFILE
```

在配置文件中添加如下设置
```bash
$OutputEncoding = [console]::InputEncoding = [console]::OutputEncoding = New-Object System.Text.UTF8Encoding
```

允许运行脚本 如果遇到脚本运行限制错误，使用管理员权限打开 PowerShell，执行以下命令
```bash
Set-ExecutionPolicy RemoteSigned
```