# Ubuntu安装Deepin-Wine



套娃Windows
<!--more-->

<details>
<summary>我是话唠</summary>
&emsp; 想把linux当做生产力，就少不了一些必要的IM软件，但是...国内的IM软件却都没有linux，之前有的web.qq和web.weixin还都被官方关闭了，虽然国外的IM全都有linux版，比如telegram，但国外有的都被墙了...唉...爱我中华，我觉得墙倒不是一个问题，就是国内的这些大厂都不争气，全民盗版windows，金钱利益催使，而国外对于正版的概念很普及，而且微软也严厉打击，所以linux的用户很多，不过现在国内正版意识逐渐建立起来，去年腾讯也拿出了新的linux2.0版QQ（那是给2020年的人用的吗？，用08年版改的）而且外加西方国家对中国的技术封锁，之前政府甚至提出让公务员都装linux，开源的linux在以后只会发展的越来越好，大势所趋吧
&emsp; 说了这么多，还是回到主题哈哈
&emsp; 在网上搜了很久，最后也只能用迫不得已且唯一的方法了，套娃一个windows的环境在linux里（真是难为我linux了）想了想，很多软件还是独占win环境的，看到网上各行各业的大佬们都在装这个环境，整就完事儿啦
&emsp; 网上各种方法的都有，各种大佬制作的wine环境，但我挨个尝试了之后，都以兼容性告别了，最后用winetricks装更是让人恼火，安装之后第一次使用没问题，关闭了之后再启动就GG了，提示少dll文件，我想我这双系统正好啊，就去windows下拷贝dll文件过来，发现没卵用，吼！！！，后来又去winetricks里那里按照网上搜到的解决方法，去禁用什么tencentplatform.exe文件，然后第二次启动QQ，原来报错三个，现在报错两个了...最后实在熬不住了，换deepin-wine试试，一直没考虑用deepin，因为感觉...不靠谱（之前装过deepin的linux，发现体验很糟糕，对deepin失去了好感）装好之后，发现...能用了....（我真的...国产系统还不错，真香...）
&emsp; 然后开始说方法吧，之前用过的那些错误方法就不提了，只说最后好使的，作为国产linux的领头羊deepin，这个wine环境官方会一直维护下去的，所以还挺靠谱的
</details>

## 1. 安装wine-deepin环境
terminal执行命令clone远程库
```bash
git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git
```
这个库很棒，作者把所有依赖库都放在了里面，然后在文件目录下执行
```
sudo ./install.sh
```
wine环境就装好了

## 2. 安装wine-QQ
[deepin-win软件池](http://mirrors.aliyun.com/deepin/pool/non-free/d/)
&emsp; 可以看到wine-deepin下支持的所有软件，然后下载QQ对应的文件，格式为deb，之后dpkg-i命令安装文件包，就可以安装成功啦，但是安装后别着急打开，我的系统是Ubuntu18.04，所以还要多下载一个Gnome的插件TopIcons Plus
***
### 2020年5月15日更新:
推荐使用TIM,QQ过于臃肿,优化不是很好
[deepin.com.qq.office](http://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.qq.office/)
***
### 2020年5月26号更新:
deepin-wine版本更新
[git仓库](https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu/)
下载仓库文件到本地,然后执行新版本的安装脚本
```
sudo sh ./install_new_version.sh
```
修复软件安装的依赖问题
```
apt-get -f install = apt-get install -f 
```
假如系统上有某个package不满足依赖条件,这个命令就会自动修复,安装那个package依赖的package。
***

## 3.安装TopIcons Plus
terminal执行命令
```bash
sudo apt install chrome-gnome-shell
```
然后打开[插件网站](https://extensions.gnome.org/extension/1031/topicons/)

&emsp; 应用该插件，随后提示下载安装，安装之后启动QQ就会顺利运行啦！撒花（虽然看起来很简单，但是真的踩了好多坑，试了很多方法...委屈哭了）

