# Linux与移动设备的文件互传


昨天在微博上碰巧看到一个分享，讲的是一个电脑与移动设备的文件互传的工具
因为Linux上的im软件一直是wine套娃，所以体验不是很好，这也是我在使用过程中心中一直不舒服的一个点，现在生产环境都已经迁移到了ubuntu，不得不说ubuntu一直让我很是满意，没有win10臃肿的系统，高度的可定制化，便捷的库安装过程，庞大的开放社区，今天上午想着试一下这个工具，安装之后使用起来真的让我感到很惊艳，在这里记录一下安装过程
<!-- more-->

## 项目地址
[github](https://github.com/claudiodangelis/qrcp)

## 安装GO环境
这个工具是go语言编写的,所以要安装一下go的环境
### 1.官网下载go语言安装包

地址：https://studygolang.com/dl

### 2.解压安装包

将下载下来的安装包解压到/usr/local下
```bash
tar xf go1.12.1.linux-amd64.tar.gz -C /usr/local
```
### 3.配置全局变量
```bash
vim ~/.bashrc
```
最后一行加上下面配置
```bash
export GOPATH=/usr/local/go
export PATH=$GOPATH/bin:$PATH
```
### 4.验证配置是否成功
```bash
go version
#warning: GOPATH set to GOROOT (/usr/local/go) has no effect
#go version go1.12.1 linux/amd64
```

### 5.出现warning，配置PATH
```bash
export GOROOT=/usr/local/go  
export GOPATH=$PATH:$GOROOT/bin  
```
### 6.测试安装

```bash
go version
#go version go1.12.1 linux/amd64
```

## 安装工具
### Install it with Go
```bash
go get github.com/claudiodangelis/qrcp
```
### **手动下载Go包的通用方法**
如果出现 https fetch: Get xxxxxxx: dial tcp: i/o timeout
那么就说明当前网络状态下载不了(被墙的太严重了)
使用手动下载就好,首先看当前下载的包的地址
```bash
github.com/${PATH1}
golang.org/x/${PATH2}  
```
若是github,则在你 src/ 目录里terminal
```bash
git  clone  https://github.com/${PATH1}  ${GOPATH}/src/github.com/${PATH1}
```
若是golang,则在你 src/golang.org/x/ 目录里使用  (在不在没关系 )
```bash
git  clone  https://github.com/golang/${PATH2}   ${GOPATH}/src/golang.org/x/${PATH2}
```
### Install the binary
下载[Release](https://github.com/claudiodangelis/qrcp/releases),将其移动到路径 /usr/local/bin 随后terminal 将其改为可执行程序
```bash
chmod +x /usr/local/bin/qrcp
```
## 工具使用
这里只举最简单的两个例子,更多用法请参照官方说明
### Send a file
```bash
qrcp MyDocument.pdf
```
### Receive files
```bash
qrcp receive
```
### 特别提示
特别提示:我在使用过程中,发现iPhone自带的扫描二维码功能,在扫描生成的二维码后,并不会跳转,似乎是iPhone的bug,这里推荐使用其他浏览器或微信进行扫描

