# 双系统安装Ubuntu18.04


厌倦了在windows里的虚拟机ubuntu来体验套娃linux，索性去重新下载了一个ubuntu18.04版本，从双系统到后来的linux命令，再到最后迁移hexo，走了很多的坑
<!--more-->

#### Win10/Ubuntu双系统的安装
```
需要的工具：
easyuefi（easybcd）
系统u盘制作软件
ubuntu镜像
```
##### 第一步：
在阿里镜像网站下载稳定版ubuntu镜像，制作启动器
在固态硬盘中创建200m的可用空间，机械硬盘中创建60-80G的可用空间（双硬盘配置的笔记本）随后u盘启动
install ubuntu，在磁盘分配界面选择自定义设置，具体参照此[bolg](https://blog.csdn.net/xrinosvip/article/details/80428133?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
注意分清swap，/boot，/ 三个分区的设定，随后安装完成，安装过程不要连接网络，否则会进行软件自动更新

##### 第二步：
重启电脑选择windows boot manager进入win10系统，打开easyuefi软件，创建新的linux启动项，选择grabx文件类型，重启电脑，即可进入双系统选择界面
此界面可以在ubuntu中美化，修改grub
之后在ubuntu中更改[镜像源](https://blog.csdn.net/xfxf0520/article/details/82975366)
此时ubuntu可以正常下载软件使用

