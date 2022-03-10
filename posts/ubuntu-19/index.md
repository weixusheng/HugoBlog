# Linux上的Apple生态圈



Apple Music Web上线啦
<!-- more-->

## AppleMusic
今天是2020年5月4号
一个值得纪念的一天
因为意外的发现

AppleMusic的网页端上线了！

原来一直都处于测试状态，而且仅限美国地区
现在国区正式上线了，我的Linux生态完整啦！！！

我好开心呜呜呜
我的运气也太好了呜呜呜
感谢命运女神一次次的眷顾我

## AppleMusic客户端
晚上逛github,发现有大佬做了一个snap-web打包出来的AM软件
好奇心!没错又是好奇心!让我下载了体验一下
在40kb下载速度的等待下,我安装好了!
体验尚可!流畅度提升了很多
[github项目地址](https://github.com/cross-platform/apple-music-for-linux)
### 安装
```bash
sudo snap install apple-music-for-linux
```

## iCloud
前几天买了iCloud的存储空间,好奇在linux上是否支持,因为之前还折腾了一下onedrive,但是失败了,所以就没抱太大希望,试着装了一个软件,发现使用体验满分!而且同步很快,支持拖拽上传文件,linux这回也有网盘了,感动到哭...
这个软件是snap格式的,所以支持命令行安装,snap是ubuntu开发的一种运行在沙盒环境的软件格式,运行速度和安全性较好
### 安装iCloud Notes Snap
```bash
sudo snap install icloud-notes-linux-client
```
安装应用程序后，打开应用程序菜单并搜索“icloud-notes-linux-client”，打开后，系统会立即提示你使用Apple ID登录Apple的iCloud系统。

### 换图标
因为软件自身的图标很是一语难尽...所以琢磨着换个图标
但是由于snap软件的文件无法进行修改,沙盒环境所限制
本来想放弃,但是尝试了最后一个方法,发现竟然好使了
修改本路径下的desktop文件
```bash
/var/lib/snapd/desktop/applications
```

