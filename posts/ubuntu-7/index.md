# Ubuntu安装Flashplayer插件


发现Firefox竟然看不了b站视频
我人又傻了，之前找了好久的方法才装上flashplayer，这次记录一下，免得下次又要铺天盖地的找方法
<!--more-->

```
- 1.启用Canonical Partners Repository存储库
最新的Flash插件位于Canonical Partners的存储库中，默认情况下它是处于禁用状态，必须在尝试安装Flash插件之前启用此存储库。

sudo add-apt-repository "deb http://archive.canonical.com/ $(lsb_release -sc) partner"

- 2.安装Adobe Flash插件

sudo apt update
sudo apt install adobe-flashplugin browser-plugin-freshplayer-pepperflash

- 3.重启Firefox
```

