# Win-科学上网



Win环境下的全局梯子配置
<!--more-->

这两天OneNote把我折腾了好久...熬了两天夜
主要问题在于,之前onenote同步数据速度很快,但现如今同步速度甚至达到了不能用的地步,忍不了鸭,我还有好多笔记在里边
所以就想解决方法,第一天晚上,知道了onenote香港cnd服务器被微软撤走了,因此国区数据同步慢(但都慢到不能用了...)
第二天就开始研究怎么不全局加速,使得数据能同步过来,一开始还天真挂香港节点,结果今天一ping,发现数据在美国微软云...我吐了
因为我买的那个加速器软件开全局会安装一个虚拟网卡,很容易出事(之前就是因为这个重装的电脑)
所以我连带着...学会了两个通用的梯子和一个全局代理工具,最后算是解决了OneNote同步问题

# SSR-酸酸乳
一开始看大家说酸酸乳,我还一脸疑惑,我在这查梯子,你们给我搞一堆饮料上来是什么操作...
后来查多了才慢慢知道,原来大佬们口口相传的酸酸乳...就是SSR
现在主流的梯子网站都会提供一个订阅地址,而一些就会提供ssr网址,当然也会有的不提供ssr,而提供其他类型的订阅网址
## SSR配置教程
### 安装
[Github项目地址](https://github.com/shadowsocksrr/shadowsocksr-csharp)

## SSR Windows客户端安装及运行
下载完成后，解压，然后后运行ShadowsocksR-dotnet2.0.exe 或 ShadowsocksR-dotnet4.0.exe 即可。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/1.png)
SSR Windows客户端的运行，需要.net环境的支持，一般来说Win7以上系统可以直接运行。
如果运行时提示错误，那么需要先安装Microsoft .NET Framework 4。
[下载地址](https://www.microsoft.com/zh-cn/download/details.aspx?id=17718)

## SSR客户端使用教程
### 1-服务器配置
1.SSR成功运行后，系统任务栏（桌面右下角时间那里，没有的话，就是隐藏起来了）会出现一个小飞机标志。
2.需要先添加SSR服务器，右键点击小飞机——服务器——编辑服务器。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/2.png)
3.在弹出的编辑服务器窗口，直接修改默认的几项参数。修改为你获取的节点数据，或直接导入ssr链接（导入链接，需要你复制链接，右键系统任务栏中的小飞机图标，点击上图中的剪贴板批量导入ssr：//链接）。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/3.png)
```
服务器IP：前面打勾后可见，需要填写你的VPS服务器IP地址
服务器端口：安装SSR服务端时设置的端口（port）
密码：安装SSR服务端时设置的密码（password）
加密、协议、混淆：这几项分别对应安装SSR服务端时设置的 Stream Cipher、Protocol、obfs 几项，下图中仅为示范，具体按实际填写
混淆参数：留空
```
### 2-添加订阅地址
如果提供是订阅链接，那么点击下图的服务器订阅——ssr服务器订阅设置
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/4.png)
在弹出的窗口点击add，把订阅网址填好后确定。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/5.png)
右键客户端，菜单中选择服务器订阅——更新服务器订阅（不通过代理）。等待更新成功的通知，然后再服务器那一栏中即可看到节点信息。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/6.png)

### 其他配置
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/7.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/8.png)

设置完成后，点击确定即可完成服务器的设置。

## 模式介绍
右键点击任务栏的小飞机，在弹出的菜单中，通过相关设置搭配，可以对访问流量进行个性化分流。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/9.png)
### 系统代理模式：
SSR的代理模式，通过该项对流量进行二次分流（第一次分流见下文）。决定进入SSR客户端的流量是否走代理。默认为全局模式，下面我们分别介绍。
1. 直连模式：
所有进入SSR的流量不走代理，相当于没有安装SSR，一般我们很少选择这一项
2. PAC模式：
通过SSR目录中的pac.txt文件，判断进入SSR客户端的流量是否走代理
3. 全局模式：
所有进入SSR客户端的流量都走代理

### PAC：
即PAC模式用到的pac.txt，在其它模式下不生效。在该菜单下，我们可以对该文件进行相关操作，一般我们选择PAC为GFWList。
在SSR服务器连接成功后，可以点击“更新PAC为GFWList”进行更新。

### 代理规则：
决定哪些流量进入SSR客户端，对电脑流量进行第一次分流。

1. 绕过局域网：局域网的IP直接连接，不进入SSR客户端。
2. 绕过局域网和大陆：局域网和大陆的IP，不进入SSR客户端，直接连接。
3. 绕过局域网和非大陆：大陆以外的IP，不进入SSR客户端，直接连接。这一项很少选择。
4. 用户自定义：很少选择。
5. 全局：所有流量全部进入SSR客户端。

## 各项目常用的搭配为：
```
系统代理模式：PAC模式。
PAC：更新PAC为GFWList。
代理规则：绕过局域网和大陆。
如何设置SSR开机启动
```
为了避免每次开机，都需要手动开启SSR，我们可以设置SSR开机自启。具体：右键点击任务栏小飞机——选项设置——开机启动打勾——确定。如下图：

## 局域网其它设备连接SSR
电脑端的SSR，其实可以通过设置，与其它局域网设备共享，实现上网。
1. 右键任务栏小飞机——选项设置——允许来自局域网的连接打勾——确定。
2. 其它局域网设备的网络连接里，设置代理地址为电脑局域网IP，端口为默认的1080。设置生效后，流量就会经过电脑的SSR进行转接了。

## 常见问题汇总
[常见问题汇总](https://www.cnblogs.com/sstealer/p/12452075.html)
[PAC配置文件介绍](https://www.dwhd.org/20160901_223415.html)

# V2rayN
## 安装
[Github项目地址](https://github.com/2dust/v2rayN)

## 使用教程
相比较SSR而言,V2rayN的使用就简单了很多,有一个图形化界面
1. 首先，找到文件夹中的v2rayn.exe文件，双击运行。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/21.png)

2. 添加订阅
点击主界面的订阅-订阅设置，然后粘贴自己的v2ray订阅地址，填写备注等信息。注意勾选备用。然后保存。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/22.png)

3. 更新订阅
点击主界面的订阅-更新订阅，等待几秒钟即可。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/24.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/25.png)

4. 启用节点
选择一个节点，单击右键-设为活动服务器，即意味着选中了此节点。（也可以在左键点击节点名称以后按一下回车键，即Enter键）
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/26.png)

5. 选择代理模式
在任务栏图标上单击右键，找到Http代理，推荐选择开启PAC，并自动配置系统代理(PAC模式)。勾选后，系统代理正式启动，已经可以达到科学上网效果。此模式下，V2RayN将会针对访问请求自动分流，国内站点直连，被屏蔽站点将走代理。（但也不是万能的，准确性非100%）
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/27.png)

6. 显示网速统计
在主界面点击参数设置，勾选启用统计，需要重启v2rayN客户端。

7. 测试服务器状态
在主界面点击测试服务状态，选择测试服务状态，即可查看真实延迟。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/28.png)

8. 更新v2rayN及v2rayCore
v2rayCore是v2ray的核心文件，v2rayN是图形化客户端。
在主界面点击检查更新，即可选择更新对象。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/29.png)

# Proxifier全局代理
酸酸乳支持全局(PAC模式和代理方式都选择全局即可)
**V2rayN全局需要搭配 Proxofier实现**
## 配置代理服务器地址
端口填写当前梯子的端口
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/31.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/32.png)

## Name Solutions
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/34.png)
只选择第二个勾选项

## 代理规则-Rules
### 指定程序代理
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/35.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-4/36.png)

### 设置全局代理
不需要在Appplications中添加程序
直接 更改Action为Proxy socks5 127.0.0.1 即可
