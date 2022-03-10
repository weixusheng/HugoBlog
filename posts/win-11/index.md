# qrcp & SwitchHosts



便捷小工具
<!--more-->

# SwitchHost

## 自动同步github host文件


scoop


<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Typora/image-20211218164204334.png" alt="image-20211218164204334" style="zoom: 50%;" />

[gitee文档地址](https://gitee.com/ineo6/hosts)

# qrcp文件传输

[Github项目地址](https://github.com/claudiodangelis/qrcp)

## 常用命令

### 发送文件

```bash
# 发送文件
qrcp MyDocument.pdf

# 发送多个文件 Multiple files
qrcp MyDocument.pdf IMG0001.jpg

# 发送一个文件夹
qrcp Documents/

# 发送一个文件的压缩版本
qrcp --zip LongVideo.avi
```

### 接收文件

```bash
# 接收文件
qrcp receive

# 接收文件到指定目录 Note: the folder must exist
qrcp receive --output=/tmp/dir
```


