# 安装scoop包管理器



scoop
<!--more-->

scoop是可用于windows的一款包管理工具，今天试着安装了一下，在这里记录一些遇到的问题和解决方法。
安装流程
先进入powershell，windows徽标键输入powershell或者cmd输入powershell均可
然后先输入以下代码，以保证后面的脚本有运行权限

```powershell
Copyset-executionpolicy remotesigned -scope currentuser
```

然后开始正式安装

```shell
Copyiex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```
