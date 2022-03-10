# OneDrive创建文件同步



OneDrive小技巧
<!--more-->

今天发现了一个小技巧...简直吹爆!
不过怎么依稀记得之前也学到过...但是一直没用起来

由于onedrive同步的文件都在一个文件夹下,因此如果想要同步单独的文件夹,只能通过`mklink`来实现

# mklink命令

## 简介
NTFS 符号链接又称“符号链接”，是 NTFS 文件系统中指向文件系统中的另一个对象的一类对象，被指向的对象叫做“目标”。mklink 是 Windows 下用于创建符号链接的工具，存在于 Windows Vista 及以后版本的 Windows 操作系统中。

## 使用方式
`MKLINK [[/D] | [/H] | [/J]] Link Target `
说明：
`/D` 创建目录符号链接而不是文件符号链接（默认为文件符号链接）
`/H` 创建硬链接而不是符号链接
`/J` 创建目录连接点
`Link` 指定新的符号链接名称
`Targe`t 指定新链接引用的路径（绝对路径或者相对路径均可）

## 注意
参数 Link 和 Target 要求不能使用 Windows 中不允许用作文件名的字符（\ / : * ? " < > |）。并且如果 Link 和 Target 这两个参数中需要包含空格，则必须使用英文双引号将内容引起来，以避免参数识别错误。

# 使用

```bash
mklink /d C:\OneDrive\File E:\File
# 将E盘下的File文件,同步到Onedrive中的File下
```

需要注意的是,在`mklink`时,`onedrive`文件下不能有`File`文件夹,该文件夹是通过`mklink`命令创建的

