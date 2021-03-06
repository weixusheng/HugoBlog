# 物联网概论 PART-6



定位技术,物联网通信体系
<!--more-->

# 定位技术
## 卫星定位与基站定位
1. 基于卫星导航的定位：
   基于卫星导航的定位方式主要是利用设备或终端上的GPS定位模块将自己的位置信号发送到定位后台来实现定位;
2. 基于参考点的基站定位：
   基站定位则是利用基站与通信设备之间无线通信和量测技术,计算两者间的距离,并最终确定通信设备位置信息。基站定位不需要设备或终端具有GPS定位功能,但是其定位精度很大程度依赖于基站的分布及覆盖范围的大小,误差较大。目前,蜂窝定位中的大部分方法都是采用基站定位实现的。

# 卫星定位技术
卫星定位导航系统是利用卫星来测量物体位置的系统。由于对科技水平要求较高且耗资巨大,所以世界上只有少数的几个国家能够自主研制卫星定位导航系统。目前已投入运行的主要包括:
 - 美国的全球定位系统(GPS),目前唯一覆盖全球的卫星定位导航系统。
 - 俄罗斯的格洛纳斯系统(GLONASS),目前只覆盖俄罗斯境内。
 - 中国的北斗导航系统(COMPASS),目前只覆盖我国境内。

## GPS
1. **发展历史**
   20世纪70年代(1973),由于人们对连续实时三维导航的需求日渐增强,美国国防部开始研究和建立新一代空间卫星导航定位系统。主要目的是为陆、海、空三大领域提供实时、全天候和全球性的导航服务。
   经过20余年的研究实验,耗资近300亿美元,到1994年3月,一个由24颗卫星组成,全球覆盖率达98%的卫星导航系统终于布设完成,该系统被称为GPS,是继阿波罗登月、航天飞机之后的第三大空间工程。
   GPS(Global Position System , 全球定位系统),全 称是“Navigation Satellite Timing And Ranging / Global PositionSystem,NAVSTAR/GPS”,其意为“导航卫星测时与测距/全球定位系统”。该系统以卫星的无线电导航技术为基础,可实现授时和测距的空间交会定点的定位导航,为全球用户提供连续、实时、高精度的三维位置、三维速度和时间等相关信息。
2. **系统组成**
   GPS系统主要由空间部分、地面控制部分和用户接收设备三部分构成
    - 宇宙空间部分
        - 24颗工作卫星
    - 地面监控部分 (全部在美国境内)
        - 1个主控中心(另有1个备用)
        - 4个专用地面天线
        - 6个专用监视站
    - 用户设备部分
        - GPS接收机
3. **GPS定位原理**
   用户设备主要是指各种型号的GPS信号接收机,由GPS接收机天线、GPS接收机主机和天线组成。其主要任务是捕获按一定卫星截止角所选择的待测卫星,并跟踪这些卫星的运行。
   GPS导航系统的基本原理是测量出已知位置的卫星到用户接收机之间的距离,然后综合多颗卫星的数据就可知道接收机的具体位置。要达到这一目的,卫星的位置可以根据星载时钟所记录的时间在卫星星历中查出。
   而用户到卫星的距离则通过纪录卫星信号传播到用户所经历的时间,再将其乘以光速得到。
4. **为什么定位同时需要四颗卫星**
   GPS导航系统卫星部分的作用就是不断地发射导航电文。
   然而,由于用户接受机使用的时钟与卫星星载时钟不可能总是同步,所以除了用户的三维坐标x、y、z外,还要引进一个Δt即卫星与接收机之间的时间差作为未知数,然后用4个方程将这4个未知数解出来。所以如果想知道接收机所处的位置,至少要能接收到4个卫星的信号。

# 基站定位
## 产生背景
GPS功能强大,但是需要专门的客户端设备才能使用,这不利于其在广泛人群中的普及。
但是,在应急服务等领域,用户往往迫切需要了解自己的地理位置。因此,一些国家开始考虑使用手机这一普及率很高的终端设备提供定位信息。产生背景: 美国通信委员会在1996年通过了增强911法案(在1999年再次修订),这个法案要求手机运营商必须知道每一部手机的地理位置(误差在50至100米误差之内)。任何一部手机拨打美国紧急服务电话911,政府就要知道其位置,即使用户自身不知道身在哪里。这一法案,迫使电信运营商开始发展手机定位系统, 基站定位技术应运而生。
 - 使用蜂窝网络
移动通信蜂窝网络是目前覆盖范围最大的无线网络,基站定位一般应用于手机用户,手机基站定位服务又叫做移动位置服务(LBS——Location Based Service),它是通过电信移动运营商的网络(如GSM网)获取移动终端用户的位置信息(经纬度坐标),在电子地图平台的支持下,为用户提供相应服务的一种增值业务。
在基站定位中,被定位移动终端通常是普通终端(手机等),这在客观上要求多个基站设备通过附加装置测量从移动终端发出的电波信号参数,如传播时间、时间差、相位或入射角等,再通过合适的定位算法推算出移动终端的大致位置。

## 定位原理
基站定位一般采用基于参考点的无线定位技术。利用移动运营商的移动通信网络,通过手机与多个固定位置收发信机之间的传播信号的特征参数来计算出目标手机的几何位置,同时,结合地理信息系统(Geographic Information System,GIS),为移动用户提供位置查询等服务。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/1.png)

## COO定位
COO(Cell Of Origin)定位是一种单基站定位,即根据设备当前连接的蜂窝基站的位置来确定设备的位置。那么很显然,定位的精度就取决于蜂窝小区的半径。(Google地图移动版)从原理上我们可以看出,COO定位其精度是不太确定的。但是这却是GSM网络中的移动设备最快捷、最方便的定位方法,因为GSM网络端以及设备端都不需要任何的额外硬件投入。只要运营商支持,GSM网络中的设备都可以以编程方式获取到当前基站的一个唯一代码。
根据手机所处的小区ID号来确定用户的位置,手机所处的小区ID号是网络中已有的信息,手机在当前小区注册后,系统的数据库中就会将该手机与该小区ID号对应起来,根据小区基站的覆盖范围,确定手机的大致位置。所以,该方法的定位精度与小区基站的分布密度密切相关。在基站密度较高的区域,这种定位方式精度可以达到100-150米左右,在基站密度较低的区域(如农村、山区),精度降到1-2公里左右。该方法的优点是定位时间短、对现有网络或手机不需要特殊要求就能够实现定位,缺点是定位精度取决于小区半径。

## 7号信令定位
1. 概念
又称为公共信道信令。即以时分方式在一条高速数据链路上传送一群话路信令的通信方式,通常用于局间。在我国使用的7号信令系统称为中国7号信令系统。为了实现电信业务的互联互通,不同通信运营商之间,特别是不同国家的运营商之间都会采用7号信令系统控制运营商之间业务交换的过程。许多的通信运营商也在自己的通信网络里面使用7号信令系统实现计费、漫游或者其他电信业务
2. 定位实现方法
该技术以信令监测为基础,能够对移动通信网中特定的信令过程,如漫游、切换以及与电路相关的信令过程进行过滤和分析,并将监测结果提供给业务中心,以实现对特定用户的个性化服务。该项技术通过对信令进行实时监测,可定位到一个小区,也可定位到地区。故适用对定位精确度要求不高的业务,如漫游用户问候服务,远程设计服务、平安报信和货物跟踪等。

## TDOA定位
基于电波到达时差TDOA ( TimeDifference of Arrival)定位与TOA定位类似,也是一种三基站定位方法。该方法是利用手机收到不同基站的信号时差来计算手机的位置信息的。如图所示,如果手机收到相邻基站BS1和BS2的信号的时间差为∆t,此时手机的位置在一条双曲线上:
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/2.png)
三个不同的基站可以测得两个TDOA(到达时差),手机位于两个TDOA决定的双线的交点上。与TOA法相似,TDOA定位方法可以采用手机到双曲线距离均方误差最小的算法,前提是有两个以上的TDOA值可以用来计算。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/3.png)

## AOA定位
AOA(Angle of Arrival,到达角度)定位是一种两基站定位方法,基于信号的入射角度进行定位。如图所示,知道了基站1到设备之间连线与基准方向的夹角α1,就可以画出一条射线L1;同样知道了知道了基站2到设备之间连线与基准方向的夹角α2,就可以画出一条射线L2。那么L1月L2的交点就是设备的位置。这就是AOA定位的基本数学原理。用函数调用表达如下。
AOA定位通过两直线相交确定位置,不可能有多个交点,避免了定位的模糊性。但是为了测量电磁波的入射角度,接收机必须配备方向性强的天线阵列。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/4.png)

## AGPS定位
1. **研发背景**
GPS虽然定位十分精确，但是其缺点也很明显：
• 硬件初始化(首次搜索卫星)时间较长,需要几分钟至十几分钟;
• GPS卫星信号穿透力若,容易受到建筑物、树木等的阻挡而影响定位精度。
AGPS定位技术通过网络的辅助,成功的解决或缓解了这两个问题。
2. **概念**
   A-GPS(AssistedGPS)网络辅助GPS定位是一种结合网络基站信息和GPS信息对手机进行定位的技术,该技术需要在手机内增加GPS接收机模块,并改造手机天线,同时要在移动网络上加建位置服务器、差分GPS基准站等设备。这种定位方法一方面通过GPS信号的获取,提高了定位的精度,误差可到10m左右;另一方面,通过基站网络可以获取到室内定位信号。不足之处就是手机需要增加相应的模块,成本较高。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/5.png)
3. **定位原理**
   A-GPS基本思想是通过在卫星信号接收效果较好的位置上设置若干参考GPS接收机,并利用AGPS服务器通过与终端的交互获得终端的粗位置,然后通过移动网络将该终端需要的星历和时钟等辅助数据发送给终端,由终端进行GPS定位测量。测量结束后,终端可自行计算位置结果或者将测量结果发回到AGPS服务器,服务器进行计算并将结果发回给终端。同时后台可获取位置信息为其它服务应用。 收灵敏度等。

# 无线室内定位
## 技术概述
室内定位系统(IPS)
 - 利用网络设备,对室内设施或用户的位置信息进行确定
 - 位置信息的类型
   - 绝对位置信息
   利用物理环境地图,对目标位置进行测量和显示
   - 相对位置信息
   依赖于目标与标定参考点之间的物理距离
   - 邻近位置信息
   对目标所属物理区域进行指定

## 技术特点
室内定位在以下方面有着自身的特点:
1. 定位精度: 更高精度的定位信息会带来更大的便利
2. 稳健性: 室内环境复杂多变,要求定位技术具有很好的自适应能力
3. 安全性: 很大一部分应用需求都是针对个人用户
4. 方向判断: 判断目标方位和未来运动趋势
5. 标志识别: 利用室内一些“标志性”目标提高定位精度
6. 复杂度: 复杂度应该较低

## 常见室内定位技术
### 红外线室内定位技术
 - 红外线室内定位技术定位的原理是,红外线IR标识发射调制的红外射线,通过安装在室内的光学传感器接收进行定位。
 - 虽然红外线具有相对较高的室内定位精度,但是由于光线不能穿过障碍物,使得红外射线仅能视距传播。
 - 直线视距和传输距离较短这两大主要缺点使其室内定位的效果很差。

### 超声波定位技术
 - 超声波测距主要采用反射式测距法,通过三角定位等算法确定物体的位置,即发射超声波并接收由被测物产生的回波,根据回波与发射波的时间差计算出待测距离,有的则采用单向测距法。
 - 超声波定位整体定位精度较高,结构简单,但超声波受多径效应和非视距传播影响很大,同时需要大量的底层硬件设施投资,成本太高。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/25.png)

### 蓝牙技术
 - 蓝牙技术通过测量信号强度进行定位。在室内安装适当的蓝牙局域网接入点,并保证蓝牙局域网接入点始终是这个微微网(piconet)的主设备,就可以获得用户的位置信息。
 - 蓝牙技术主要应用于小范围定位,设备易于集成在 PDA、PC以及手机中。
 - 持有移动终端设备的用户,只要设备的蓝牙功能开启,蓝牙室内定位系统就能够对其进行位置判断
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/24.png)

### 射频识别技术
 - 射频识别技术利用射频方式进行非接触式双向通信交换数据以达到识别和定位的目的。
 - 这种技术作用距离短,一般最长为几十米。但它可以在几毫秒内得到厘米级定位精度的信息。同时由于其非接触和非视距等优点,可望成为优选的室内定位技术。
 - 优点是标识的体积比较小,造价比较低,但是作用距离近,不具有通信能力,而且不便于整合到其他系统之中。

### Wi-Fi技术
 - 概念：是无线局域网络系列标准之IEEE802.11的一种定位解决方案。该系统采用经验测试和信号传播模型相结合的方式,需要很少基站,系统总精度高。
 - 起源：Wi-Fi 定位技术源于 90 年代末在美国兴起的热点地图,热点地图最初的目的是为了大家上网方便,国外许多公司都推出了允许用户在地图上查看(或标记)无线热点位置的应用程序。

## 室内定位算法
### 质心定位算法
质心定位计算法进行定位的过程是通过在定位区域内位置定位准确的且一定数量的参考点,定位目标设备通过收集参考点发出的信号并传输至服务器进行计算以确定定位目标设备是否处于该定位区域,如果定位目标设备处于定位区域,则根据参考点组成的多边形质心位置确定定位目标的具体位置。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/26.png)
质心定位算法作为一种非测距的定位算法主要是依靠参考节点的位置信息发送给未知节点,未知节点通过邻居节点组成的多边形的质心来确定自身的估计位置。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/27.png)

### 信号指纹定位算法
位置指纹识别计算法首先需要确定定位区域内的特征点位置,使特征点位置具有一定的规则性,并收集特征点发出的AP信号强度也就是RSSI值,并建立AP位置信号RSSI值数据库。在实际定位过程中,通过对定位目标设备收集到的APP信号强度进行分析,并与数据库中现有的AP信号进行对比,选择与目标设备AP信号特征相似度最高的信号位置作为目标设备的最终定位位置。在基于 RSSI 测距技术的定位系统中,已知发射节点的发射信号强度,接收节点根据接收到信号的强度,计算出信号的传播损耗,利用理论模型将传输损耗转化为距离,再利用已有的算法计算出节点的位置。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/28.png)
式中,A表示距发射节点1m处的信号强度指示值的绝对值;d表示发射节点与接收节点之间的距离;n为与环境相关的路径损耗系数。

# 物联网通信体系
## 分类
 - 无线通信技术
   - 短距离通信:蓝牙、NFC
   - 中距离无线:WIFI、Zigbee
   - 远距离无线:3G、4G、5G
   - 长距离无线:微波、卫星
 - 有线通信技术
   - 双绞线、光纤组网

### 有线通信
 - 有线通信是指利用金属导线、光纤等有形媒质传送信息的技术。
 - 有线网络示意图
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/6.png)

### 无线通信方式
 - 无线通信是利用电磁波信号在空间中直接传播而进行信息交换的通信技术,无需有形的媒介连接。

### 短距离通信与长距离通信
把通信距离在100m以内的称之为短距离通信
把通信距离超过1000m的称之为长距离通信
 - 短距离通信场景:在家里,办公室,宾馆等
常用技术:蓝牙、NFC、WIFI、ZigBee(无线)
蓝牙、NFC 传输距离 < WIFI、ZigBee 传输距离
双绞线以太网(有线)
 - 中远距离通信场景:一个城市的不同地区、不同城市之间等
常用技术:3G、4G、5G、微波、卫星等(无线)
光纤网(有线)

# WIFI
## 技术标准
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/7.png)

## 基本概念
 - WiFi局域网的本质特点
不使用通信电缆将计算机与网络进行连接,而是用无线的方式,从而使网络的构建和终端的移动更加灵活。
 - WiFi局域网两个重要的基本概念
   - **站点(Station,STA)**:每一个连接到无线网络中的终端(如笔记本电脑、手机等可联网的设备)都称之为一个站点。
   - **无线接入点(Access Point,AP)**:无线网络的创建者,也是网络的中心节点。一般家庭或办公室使用的无线路由器就一个AP。

## 网络拓扑结构
1. WiFi无线网络包括两种类型的拓扑形式:
 - 基础网(Infrastructure)
 - 自组网(Ad-hoc)
2. 两者的差别:
 - 基础网基于AP组建
 - 自组网中仅含有STA,不存在AP

### WiFi组网:基础网
 - 基于AP组建的基础无线网络
 - 由AP创建,众多STA加入所组成
 - AP是整个网络的中心
 - 各STA间不能直接通信,需经AP转发
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/8.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/9.png)
现在,AP 和ADSL Modem（俗称的“猫”）往往合二为一
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/10.png)

### WiFi组网:自组网
Ad-hoc模式也称对等模式,允许一组具有无线功能的计算机或移动
设备之间为数据共享而迅速建立起无线连接。
 - 仅由两个及以上STA组成,网络中不存在AP
 - 各设备自发组网,设备之间是对等的
 - 网络中所有的STA之间都可以直接通信,不需要转发
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/11.png)

## WiFi网络的安全机制
 - 与有线网络不同,理论上无线电波范围内的任何一个站点都可以监听并登录无线网络,所有发送或接收的数据、都有可能被截取。为了使授权站点可以访问网络而非法用户无法截取网络通信,无线网络安全就显得至关重要。
 - 安全性主要包括访问控制和加密两大部分
  1. 访问控制：保证只有授权用户才能访问敏感数据
  2. 加密： 保证只有正确的接收方才能理解数据

### 用户接入过程中的认证
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/12.png)
 - 认证是STA向AP证明其身份的过程
 - 只有通过身份认证的站点才能进行无线接入访问
 - 认证可以通过MAC地址进行,也可以通过用户名/口令进行
 - 认证有开放系统认证和共享密钥认证两种
 - STA和AP均可通过解除认证来终结认证关系

### 认证系统
 - 开放系统认证:不需要认证,没有任何安全防护能力
 - 共享密钥认证
   1. STA向AP发送认证请求
   2. AP随机产生一个challenge包(即一个字符串)发送给STA
   3. STA将收到的字符串拷贝到新消息中,用密钥加密后再发送给AP
   4. AP接收到该消息后,用密钥将该消息解密,然后对解密后的字符串和最初给STA的字符串进行比较。相同则通过认证,不相同则认证失败。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/13.png)

# 蓝牙技术
## 概念
 - 蓝牙技术是一种支持设备短距离通信(一般几十米内)的技术。
 - 蓝牙技术采用分散式网络结构,支持点对点及点对多点通信 。
 - 工作在2.4~2.485GHz频段,其数据速率为1Mbps。采用时分双工传输方案实现全双工传输。
 - 能在包括移动电话、无线耳机、笔记本电脑等等众多设备之间进行无线信息交换。

## 蓝牙技术组网
 - 蓝牙系统采用一种灵活的、无基站的组网方式,使得
一个蓝牙设备可同时与 7 个其他的蓝牙设备相连接。
 - 蓝牙系统的网络结构的有两种形式:
   - 微微网(Piconet)
   - 散射网(Scatternet)

### 微微网结构
 - 微微网是通过蓝牙技术以特定方式连接起来的微型网络,一个微微网可以只是2台相连的设备,也可以是8台相连的设备。
 - 在一个微微网中,所有设备的级别都是相同的,有相同权限。
 - 蓝牙采用自组式组网方式(Ad hoc),微微网由主设备单元(发起链接的设备)和从设备单元构成,有1个主设备单元和最多7个从设备单元。

### 散射网结构
 - 散射网由多个独立的、非同步的微微网组成,以特定的方式连接在一起。
 - 一个微微网中的主设备单元同时也可以作为另一个微微网中的从设备单元,作为 2 个或 2 个以上微微网成员的蓝牙单元就成了网桥(bridge)节点 。
 - 网桥最多只能作为一个微微网的主设备,但可以作为多个微微网的从设备。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/14.png)

## 主设备和从设备
 - 主设备一般具有输入端。在进行蓝牙匹配操作时,用户通过输入端可输入随机的匹配密码来将两个设备匹配。蓝牙手机、安装有蓝牙模块的PC等都是主设备
 - 从设备一般不具备输入端。因此从设备在出厂时,在其蓝牙芯片中,固化有一个4位或6位数字的匹配密码。蓝牙耳机、UD数码笔等都是从设备。
   - 例如:蓝牙PC与蓝牙耳机匹配时,用户将蓝牙耳机上的蓝牙匹配密码正确的输入到蓝牙PC上,完成两者之间的匹配。

## 蓝牙协议栈
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/15.png)
 - 物理层
负责提供数据传输的物理通道(通常称为信道)。一个通信系统中往往存在几种不同类型的信道,如控制信道、数据信道、语音信道等等。
 - 数据链路层
在物理层的基础上,提供两个或多个设备之间、和物理无关的逻辑传输通道(也称作逻辑链路)。
 - 中间件层
提供针对某种通信方式、服务模式的协议。比如针对语音通信的协议、针对串口通信的协议、查询服务的协议等。
 - 应用层
为实现各种各样的应用功能制定的协议。

## 蓝牙技术安全模式
1. 非安全模式
2. 业务层安全模式
3. 链路层安全模式

 - **业务层安全模式**
该模式的安全机制对系统的各个应用和服务需要进行分别的安全保护,包括授权访问、身份鉴别和加密传输。
 - **链路层安全模式**
链路层安全机制对所有的应用和服务的访问都需要访问授权、身份鉴别和加密传输。

业务层安全模式与链路层安全模式的本质区别在于:
• 业务层安全模式下的蓝牙设备在信道建立以后启动安全性过程,
即其安全性过程在较高层协议进行;
• 链路层安全模式下的蓝牙设备在信道建立以前启动安全性过程,
即其安全性过程在低层协议进行。

# ZigBee技术
## 概念
 - ZigBee 是 基 于 IEEE802.15.4 标 准 的 低 功 耗 局 域 网 协 议 ,ZigBee技术是一种近距离、低复杂度、低功耗、低速率、低成本的双向无线通讯技术。
 - 主要适合用于自动控制和远程控制领域,可以嵌入各种设备。非常适合用于有周期性数据、间歇性数据和低反应时间数据传输的应用。
 - ZigBee又称紫蜂协议,来源于蜜蜂的八字舞(蜜蜂通过之字形“舞蹈”与同伴传递信息)。也就是说ZigBee协议可以建立类似于蜂群的通信网络。
 - ZigBee是一个由可多到65000个模块组成的无线网络平台,任何结点之间可以相互通信,通信距离一般在几十米左右。

## 技术特点
 - 低功耗:在低耗电待机模式下,2节5号干电池可支持1个节点工作6~24个月,甚至更长。相比较,蓝牙能工作数周、WiFi可工作数小时。
 - 低成本:通过大幅简化协议,降低了对通信控制器的要求,每块芯片的价格远低于蓝牙或WiFi。
 - 低速率:ZigBee 分别提供 250kbps(2.4GHz)、40kbps (915 MHz)和 20kbps(868 MHz)的原始数据吞吐率,满足低速率传输数据的应用需求。
 - 短时延:ZigBee的响应速度较快,一般从睡眠转入工作状态只需15ms,节点连接进入网络只需30ms,进一步节省了电能。相比较,蓝牙需要3~10s、WiFi 需要3s。
 - 高安全:ZigBee提供了三级安全模式,包括无安全设定、使用访问控制清单(ACL) 防止非法获取数据以及在数据转移中采用高级加密标准(AES)的对称密码加密。

## ZigBee网络组成
1. 协调器
在ZigBee 网络中,有且只能有一个协调器,它在网络中起了网络搭建和网络维护的功能。是整个网络的中心枢纽。是等级最高的父节点。
2. 路由器
路由器在ZigBee 网络中既可以充当父节点,也可以充当子节点,有信息转发和辅助协调器维护网络的功能。
3. 终端
终端的功能最为简单,只能加入网络,为最末端的子节点设备。只能与其父节点进行通信,如果两个终端之间需要通信,必须经过父节点进行多跳或者单跳通信。是网络中数量最多的节点,也是低功耗的网络设备。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/17.png)

## 网络拓扑结构
### 星状网络
星状网络包含一个协调器(中心节点)和若干个路由器和终端(附属节点)组成。该结构如下图所示:
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/18.png)
该结构网络中,每个附属节点只能与中心节点通信,如果需要两个附属节点之间通信,必须经过中心节点进行数据转发。

### 树状网络
树状网络包含一个协调器,若干个路由器和终端组成。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/19.png)
结点之间的信息只能沿着树的路径向上传递到共同的父节点,再由共同的父节点向下转发给目的节点。

### 网状网络
网络中具有路由功能的设备之间都可以直接相互通信,在通信范围内不需要其他节点转发。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/20.png)
网络中每个结点具有重新选择路由的功能,所以当某个路由器出现故障时,网络可以**自动重新组网**,因此该组网方式良好的可靠性。

## ZigBee协议栈
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/21.png)

### ZigBee网络协议各层功能
 - 物理层(PHY)
物理层定义了物理无线信道和MAC子层之间的接口。物理层数据服务从无线物理信道上收发数据。
 - MAC层
MAC 层负责处理所有的物理无线信道访问,并产生网络信号、同步信号;提供两个对等MAC实体之间可靠的链路 。
 - 网络层
网络层主要实现结点加入或离开网络、接收或抛弃其他结点、路由查找及传送数据等功能,支持多种路由算法。
 - 应用层
应用层包含三部分:应用框架AF、ZigBee设备对象ZDO和应用支持APS子层。它们实现了将不同的应用对象映射到ZigBee网络层 。

## ZigBee应用领域
 - 家庭和建筑物的自动化控制:照明、空调、窗帘等家具设备的远程控制;
 - 消费性电子设备:电视、DVD、CD机等电器的遥控。
 - 工业控制:使数据的自动采集、分析和处理变得更加容易;
 - 医疗设备控制:医疗传感器、病人的紧急呼叫按钮等;

## Zigbee、蓝牙、WIFI对比图
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/16.png)

# 移动通信技术
## 概念
 - 移动通信是指通信的双方,或至少有一方处于移动状态下进行信息交换的通信就叫做移动通信。移动体可以是人,也可以是汽车、火车、轮船等在移动状态中的物体。
 - 移动通信系统包括无绳电话,无线寻呼,陆地蜂窝移动通信,卫星移动通信等。一般情况下,移动通信系统是指陆地蜂窝移动通信系统。
 - 移动通信除了依靠无线通信技术之外,还依赖有线通信网络的支持,如公众电话网PSTN,公众数据网PDN, 综合业务数字网ISDN。

## 蜂窝通信网络
### 概念
20世纪70年代中期,贝尔实验室发明了蜂窝式组网技术。
 - 这种技术放弃了点对点传输和广播覆盖模式,把整个服务区域划分成若干个较小的区域(蜂窝技术因此而得名)。
 - 各小区均用小功率的发射机(即基站发射机)进行覆盖,许多小区像蜂窝一样能布满 (即覆盖)任意形状的服务地区。手机均采用这项技术,因此常常被称作蜂窝电话。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/22.png)

## 蜂窝移动通信系统组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-6/23.png)
 - 移动台(MS)是用户使用的移动终端设备,最常见的是手机。
 - 基站(BS)的作用是实现覆盖范围内的移动终端与移动通信系统之间的无线通信。
 - 移动业务交换中心(MSC)的主要任务是实现服务区内各个小区之间的信息交换、服务区内外的信息交换和对服务区内各个小区进行集中控制管理。
  
## 蜂窝移动通信特点
### 频率复用
 - 每一组连接(对于无线电话而言就是一组会话)都需要专门的频率,而可以使用的频率一共只有大约1000个。
 - 为了使更多的会话能同时进行,蜂窝系统把给每一个“蜂窝”(即每一个小的区域) 分配了一定数额的频率。不同的蜂窝可以使用相同的频率。这样有限的无线资源就可以充分利用了。

### 越区切换
越区切换通常发生在移动台从一个基站覆盖的小区进入另一基站覆盖小区的情况下,为保持通信的连续性,将移动台与当前基站之间的链路转移到移动台与新基站之间的链路。

### 位置登记
移动系统中,用户在系统覆盖范围内任意移动。为能把一个呼叫传送给移动的用户,必须有一个高效的位置管理系统来跟踪用户的位置变化。
现有的移动系统中,位置管理采用数据库技术实现。

# 蜂窝网络的发展
## 第一代:模拟语音
 - 第一代移动通信技术主要有三种窄带模拟系统标准:
   - 北美蜂窝系统AMPS
   - 北欧移动电话系统NMT
   - 英国的全接入通信系统TACS
我国采用的主要是TACS制式,即频段为890 ~ 915MHz与935 ~ 960MHz
 - 第一代移动通信的各种蜂窝网系统有很多相似之处,但是也有很大差异,它们只能提供基本的语音会话业务,不能提供非语音业务,并且保密性差。
 - 不同系统之间还互不兼容,移动用户无法在各种系统之间
实现漫游。

## 第二代:数字语音
第二代移动通信技术使用数字制式,支持传统语音通信、文字和多媒体短信,并支持一些无线应用协议。主要有如下二种工作模式:
1. GSM移动通信(900/1800MHz)
    - GSM是应用最为广泛的移动电话标准。GSM有以下重要特点:防盗拷能力佳、网络容量大、手机号码资源丰富、通话清晰、稳定性强不易受干扰、信息灵敏、通话死角少、手机耗电量低、机卡分离 。
    - GSM 被设计具有中等安全水平。系统设计使用共享密钥用户认证。用户与基站之间的通讯可以被加密。
2. CDMA移动通信(800MHz)
    - 工作在800MHz频段,核心网移动性管理协议采用IS-41协议,无线接口采用窄带码分多址(CDMA)技术。
    - CDMA在蜂窝移动通信网络中的可用容量在理论上可以达到AMPS容量的20倍。CDMA可以同时区分并分离多个同时传输的信号。
    - CDMA有以下特点:抗干扰性好、抗多径衰落、保密安全性高、容量质量之间可以权衡取舍、同频率可在多个小区内重复使用。

## 第三代:语音与数据
第三代移动通信技术(3G),是指支持高速数据传输的蜂窝移动通讯技术。
 - 3G通信可以提供所有2G的信息业务,同时保证更快的速度,以及更全面的业务内容,如移动办公,视频流服务等。
 - 3G的主要特征是可提供移动宽带多媒体业务,包括高速移动环境下支持144Kbps速率,步行和慢速移动环境下支持384Kbps速率,室内环境则应达到2Mbps的数据传输速率,同时保证高可靠服务质量。

三种主流的3G标准分别是CDMA2000,TD-SCDMA,W-CDMA。

### CDMA2000
CDMA2000由美国高通北美公司为主导提出。这套系统是从窄频CDMAOne数字标准衍生出来的,可以从原有的CDMAOne结构直接升级到3G,建设成本低廉。使用的地区只有日、韩和北美。CDMA2000与另两个主要的3G标准WCDMA以及TD-SCDMA不兼容。

### W-CDMA
 - WCDMA是基于GSM网发展出来的3G技术规范,是欧洲提出的宽带CDMA技术。
 - WCDMA采用最新的异步传输模式(ATM)微信元传输协议,能够允许在一条线路上传送更多的语音呼叫,呼叫数由30个提高到300个,在人口密集的地区线路将不再容易堵塞。
 - 另外,WCDMA还采用了自适应天线和微小区技术,大大地提高了系统的容量。

### TD─SCDMA
 - TD-SCDMA是由我国信息产业部电信科学技术研究院提出,与德国西门子公司联合开发。
 - 主要技术特点:同步码分多址技术,智能天线技术和软件无线技术。采用TDD双工模式,载波带宽为1.6MHz。TDD是一种优越的双工模式,能使用各种频率资源,能节省未来紧张的频率资源,而且设备成本相对比较低。
 - 另外,TD─SCDMA独特的智能天线技术,能大大提高系统的容量,而且降低了基站的发射功率,减少了干扰。

## 第四代移动通信技术(4G)
### 概念
 - 4G是集3G与WLAN于一体能够传输高质量图像、视频的技术产品。
 - 4G系统能够以100Mbps的速度下载,上传的速度也能达到20Mbps。
 - 价格方面,4G与固定宽带网络在价格方面不相上下。
 - 国际电联认可的4G标准包括: WiMax、WirelessMAN-Advanced、HSPA+、LTE、LTE-Advanced。

### WiMax-全球微波接入(802.16无线城域网)
1. 优势:
   - WiMax所能提供的最高接入速度是70Mbps,是3G所能提供的宽带速度的30倍。
   - 实现更远的传输距离。
2. 劣势:
   - WiMax技术标准不能支持用户在移动过程中无缝切换。
   - WiMAX严格意义讲不是一个移动通信系统的标准,而是一个无线城域网的技术。
3. 涉及技术:
   - 正交频分多址(OFDMA)、多I/O(MIMO)智能天线技术

### WirelessMAN-Advanced
即 IEEE 802.16m 标准(WiMax的升级版)
1. 优势:
    - 支持用户在移动过程中无缝切换。
    - 有5种网络数据规格,功耗节省。
    - 可在“漫游”模式或高效率/强信号模式下提供1Gbps的下行速率。
2. 涉及技术 :
    - 正交频分多址(OFDMA)、多I/O(MIMO)智能天线技术

### HSPA+ 技术
1. 优势:
    - 是一种经济而高效的4G网络,成本上的优势很明显。
    - 是商用条件最成熟的4G标准。
2. 劣势:
    - 速度一般,比不上其他4G标准。
3. 涉及技术:
    - 新的传输信道E-DCH、新的物理信道E-DPDCH

### LTE(长期演进)技术
1. 优势:
    - 下行峰值速率为100Mbps、上行为50Mbps。
    - 强调向下兼容,支持已有的3G系统和非3GPP规范系统的协同运作。
2. 劣势:
    - LTE终端设备有耗电较大。
3. 涉及技术:
    - 基于TDD的双工技术、OFDM(正交频分复用技术)、基于MIMO/SA的多天线技术

### LTE-Advanced 技术
1. 优势:
   - 峰值速率:下行1Gbps,上行500Mbps 。
   - 后向兼容的技术,完全兼容LTE,是演进而不是革命。
2. 劣势:
   - 密集部署、重叠覆盖会造成很复杂的干扰。
3. 涉及技术:
   - 基于TDD的双工技术、OFDM(正交频分复用技术)、基于MIMO/SA的多天线技术

## 4G技术的特点
4G系统主要是以**正交频分复用**为技术核心。其主要思想是:将信道分成若干正交子信道,将高速数据信号转换成并行的低速子数据流,调制到在每个子信道上进行传输。

 - **智能天线**应用数字信号处理技术,产生空间定向波束,使天线主波束对准用户信号到达方向,旁瓣或零陷对准干扰信号到达方向,达到充分利用移动用户信号并消除或抑制干扰信号的目的。这种技术既能改善信号质量又能增加传输容量。
 - **mimo(多输入多输出)**技术是指利用多发射、多接收天线进行空间分集的技术,它采用的是分立式多天线,能够有效的将通信链路分解成为许多并行的子信道,从而大大提高容量。

