# Ubuntu输入法配置


本来今天要去净月潭,不过外面阴天,只好窝在家里了...
打开电脑改没写完的blog,感觉再继续用 ibus 我的眼睛就真的要废掉了
所以下定决心
再重新装fcitx!!!(我就不信了)
根据之前的失败经历，我想这次就装个fcitx-google-pinyin就好
经过n次重启之后，终于装上了，更让我意外的是，sogou竟然也好使了！
ubuntu下的ibus和fcitx有很大的坑，两个输入法共同存在，会引起冲突
而且ibus还不能完全卸载，只能通过开机配置文件去限制他的启动
**填坑上方法**
<!-- more-->

## 1.安装fcitx
```bash
$ sudo apt install fcitx
```
输入命令
```bash
$ im-config
```
im-config是Input Method Configuration的缩写

运行如下命令之后，就会开启 Input Method Configuration 窗口。
在依次出现的窗口中，依次选择OK --> Yes --> fcitx --> OK
然后重启电脑

## 2.安装Google拼音

先安装googlepinyin
```bash
$ sudo apt install fcitx-googlepinyin
```
然后，运行如下命令
```bash
$ fcitx-config-gtk3
```
依次进行如下操作

 - 打开Input Method Confuguration窗口；
 - 点击 + 号；
 - 取消勾选「 Only Show Current Language 」这个选框；
 - 在搜索栏中输入 google，就会找到我们刚才安装的Google Pinyin这个输入法，选中它，点击「 OK 」，就把谷歌拼音输入法添加到当前输入法列表中了；

## 3.屏蔽ibus
此时如果发现fcitx中语言选择为空,就需要一些神奇的操作了
根据来自 fcitx 的[这篇文档](https://fcitx-im.org/index.php?title=Note_for_GNOME_Later_than_3.6&oldid=2312)，我们了解到需要进行一些环境的改动。

首先修改一下 gnome-settings-daemon 的设置：
```bash
$ gsettings set org.gnome.settings-daemon.plugins.xsettings overrides "{'Gtk/IMModule':<'fcitx'>}"
$ gsettings set org.gnome.settings-daemon.plugins.keyboard active false
```
第一行是修改输入法模块，第二行是禁用键盘动作。
编辑家目录下的.xprofile文件，如没有则创建,添加以下内容：
```bansh
export XMODIFIERS=@im=fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
```

3.编辑家目录下的.xinitrc文件，如没有则创建,添加以下内容：
```bash
#!/bin/sh
#
# ~/.xinitrc
#
# Executed by startx (run your window manager from here)

if [ -d /etc/X11/xinit/xinitrc.d ]; then
for f in /etc/X11/xinit/xinitrc.d/*; do
[ -x "$f" ] && . "$f"
done
unset f
fi

eval `dbus-launch --sh-syntax --exit-with-session`
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

exec startxfce4
```

最后重启电脑,就可以使用fcifx了

## Ubuntu20.04下的搜狗拼音
&emsp;由于一些迷惑原因,优麒麟20.04版本的商店上架了搜狗linux版,但是目前搜狗官网没有上架20.04的安装包,所以个人猜测应该是有一些内部交易,希望国内用户转向优麒麟社区,不过...我还是算了吧,国内社区也就那样,UI设计还是没法看,感觉都不知道抄的Win10还是OSX,还是原生Ubuntu舒服

所以这里安装优麒麟Ubuntu社区特供版的搜狗

[下载地址](https://gitee.com/laomocode/fcitx-sogouimebs/releases)
```bash
sudo dpkg -i sogouimebs.deb
```
解决依赖
```bash
sudo apt --fix-broken install
```
重新安装
```bash
sudo dpkg -i sogouimebs.deb
```
然后就可以使用了

