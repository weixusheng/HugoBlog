# Ubuntu20.04安装wine


今天想在ubuntu上装一个cajviewer,去下了一个deepin-wine版本的,但是缺少字体库装不上
去晚上找了很多帖子,都推荐去装wine,一开始疑惑wine和deepin-wine会不会发生依赖库冲突
后来去deepin的官方帖子看到管理员说不冲突,我才放心去折腾,嘿嘿
这里以ubuntu20.04版本作为基准,以下版本不适用
>使用后发现,对于win的程序,deepin-wine体验会更好,wine可作为补充使用
<!-- more-->

# 安装wine
1. 如果系统是64位，请启用32位体系结构（如果尚未安装）：
```
sudo dpkg --add-architecture i386 
```
2. 下载并添加存储库密钥：
```
wget -O - https://dl.winehq.org/wine-builds/winehq.key | sudo apt-key add -
```
3. 添加存储库（按照ubuntu的版本执行相应的命令）：
Ubuntu 20.04 	
```
sudo add-apt-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ focus main'
```
4. 更新软件源
```
sudo apt update
```
5. 安装wine5.0稳定版（winehq-stable稳定版，wine–devel开发版）：
```
sudo apt install --install-recommends winehq-stable
```
6. 打开终端，输入wine --version，如果出现“wine-5.0”，说明安装成功了！！

# 安装winetricks
打开终端，先更新apt源
```
sudo apt update
```
然后安装winetricks：
```
sudo apt winetricks
```

# wine的使用
wine运行exe软件，实际上有两种方法
一种是在终端上打开软件(不推荐)
```
wine name.exe
```
第二种方法是直接用归档管理器或文件系统直接打开exe文件。如打开软件根目录，找到主执行程序文件，右键选择以"Wine Windows Program Loader"方式打开软件

# 解决软件乱码
1. 下载以下链接文件，压缩包Fonts.zip里包含了所有Windows的字体。
```
链接: https://pan.baidu.com/s/1SWTe1Dj485FTJSdKqI6QCA
密码: 4abj
```
2. 解压该压缩包
```
unzip Fonts.zip
```
3. 把Fonts文件夹内所有字体复制到wine的映射目录内
```
cp Fonts/* ~/.wine/drive_c/windows/Fonts/
```

# 解决软件输入框不显示字体的问题
```
winetricks riched20
```

最后感谢一个大佬的[帖子](https://blog.csdn.net/cxrshiz7890/article/details/106038424)
