# Ubuntu安装NVIDIA驱动


一个月前我被N卡折磨的重装系统,一个月后的今天,本小辣鸡又来了!
跟上次雪崩不同的是,这次是成功的!记录一下过程
<!-- more-->

早上看到知乎热搜,竟然有我之前玩的那个沙盒游戏<Besiege>
因为之前之前那个游戏实在win环境下玩的,不由得想起来,似乎现在steam上支持Linux的游戏越来越多了,好奇心让我去steam官网看了一下,没想到竟然真的支持Linux!!!
随后一波操作下载steam,安装,启动
可是!为什么游戏只有...30fps...虽然画面流畅度还算凑活,但是...这也太难受了吧,想到之前win环境下运行很流畅,心中不禁有很多问号,难道是游戏优化问题?不能啊,是不是显卡用的核显? emmmmm....好像是
到网上查了一下,发现确实,在系统详情里会显示当前使用的显卡,我点开一看,果然是intel核显...吼!
看了很多教程,和blog,小心翼翼的一步步操作(真的害怕系统雪崩)
最后在这里记录一下过程,想必以后也会再装的,给未来的自己填个坑(我也太好了)
## 1. 检查电脑有哪些显卡
执行Terminal命令
```bash
lshw -c video
```
![img1](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/ubuntu-14/1.png)

## 2. 查看可安装驱动
Ubuntu自带了为Nvidia显卡开发的开源Nouveau驱动,这个Nouveau驱动是包含在Linux内核中的。但是它不支持3D加速。为了获得最佳图形性能，这里用 software-properties-gtk 这个程序来安装专有的Nvidia显卡驱动。在 terminal 里输入下面的命令
```bash
software-properties-gtk
```
![img2](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/ubuntu-14/2.png)

该命令将打开软件与更新窗口。点击"额外驱动"标签。可以看见Ubuntu默认为Nvidia显卡启用了Nouveau开源驱动，并且列出了可以安装的专有显卡驱动。
## 3. 查看推荐驱动并安装
在终端里输入下面的命令来查看哪一个专有驱动是推荐安装的。
```bash
 sudo ubuntu-drivers devices
 ```
 ![img3](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/ubuntu-14/3.png)

可以看到nvidia-driver-435是推荐安装的驱动.在终端里输入下面的命令安装这个驱动。
```bash
sudo apt-get install nvidia-driver-435
```
## 4. 安装过程中的设置密码
下载完成后,会显示需要输入一个密码,以确认驱动安装,并且重启开机时会让重新输入一次,值得注意的是,开机时会有四个选项,选择第二个**enroll MOK**
然后输入密码进入系统,这时打开设置,就会看到!安装完成啦

![img4](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/ubuntu-14/4.png)

## 5. 快乐切换显卡
打开程序列表,会发现N卡的软件,在软件中,可以切换当前显卡

![img5](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/ubuntu-14/5.png)

然后打开游戏,就会有发现...真的有这么丝滑吗

![img6](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/ubuntu-14/6.png)

最高140fps的体验,这就是Linux下的游戏吗
 i 了

