# Ubuntu管理已安装的包



Synaptic Package Manager
<!-- more-->

## 包管理软件
在Ubuntu中安装了很多包之后，很难管理，而且对于我这种强迫症...实在忍不了
搜了一会发现一个好用的可视化管理工具

>Synaptic Package Manager

安装很简单，打开终端，输入以下命令：
```bash
sudo apt-get install synaptic
```
中文系统安装之后显示“新立得软件包管理器”

## 搜狗输入法配置文件
另外今天想设置搜狗输入法，但是之前把状态栏隐藏了，又因为安装的是优麒麟社区特供版，所以导致现在调不出搜狗的设置，一番重装之后发现还是不行，感觉应该是有配置文件，果然在 /home/username/.config/SogouShell/usr/PCPYDict 找到了
env.ini 配置文件
```bash
StatusAppearance=1 #设置为1即可
```
