# WSL迁移



WSL默认安装在C盘,因此有必要迁移出系统盘
<!--more-->

# 下载工具包
[Github下载ZIP包](https://github.com/DDoSolitary/LxRunOffline/releases)

# 解压
解压之后 在解压之后的目录运行
```bash
.\LxRunOffline.exe list #查看已经安装的WSL
```

# 迁移
```bash
.\LxRunOffline.exe move -n Ubuntu -d D:\ubuntu  #安装的WSL 迁移到指定目录
```

# 迁移完成
```bash
.\LxRunOffline.exe get-dir -n Ubuntu #可以看到新的目录
```
