# 物联网概论 PART-8



网络体系结构
<!--more-->

# 网络体系结构
## OSI七层参考模型
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/1.png)

## 接口与协议
 - 每个分层负责为自己的上一层提供特定的服务,上下层之间进行交互所遵循的约定叫做“接口”
 - 同一层之间的交互所遵循的约定叫做“协议”

## 网络分层的形象类比
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/2.png)

# 网络协议
 - 协议是为进行网络中的数据交换(通信)而建立的规则、标准或约定。
 - 两个通信对象对等层之间使用同样的协议才能互相交流。网络体系结构的每个层次都有各自的协议。例如:
    - 应用层协议有http(万维网)、ftp(文件传输)、smtp(邮件)等;
    - 物理层协议有IEEE 802.3(以太网)、IEEE 802.11(WIFI)、IEEE 802.15.4 (Zigbee) 等。

## TCP/IP
在实际应用中,由于OSI七层结构过于复杂,人们一般都使用TCP/IP的四层结构。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/3.png)
 - 应用层:为用户的应用进程提供服务。制定了各种软件应用的协议。
 - 传输层:为两台主机之间提供端到端的通信。它是一个逻辑通道,负责传输的可靠性。
 - 网络层:处理分组交换网上的不同节点间的数据传输工作。主要是提供了地址管理、路由选择服务。
 - 网络接口层:提供物理层面上不同节点间的连接和数据传送服务。制定了各种网络设备的传输协议。

## 数据封装与解封
主机A中数据包向下传递时逐层封装,达到主机B后,向上传递时逐层解封。通信双方对等层要遵循同样协议。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/4.png)

## 分层网络体系的优点
 - 各层之间是独立的。某一层并不需要知道它的下一层是怎样实现的,仅需要知道该层通过层间的接口所提供的服务。
 - 灵活性好。当某一层发生变化时,只要层间接口关系保持不变,则在这层以上或者以下各层均不受影响。
 - 易于实现和维护。这种结构使得实现和调试一个庞大而又复杂的系统变得易于处理,因为整个系统已被分解为若干个相对独立的子系统。

# 传输层和网络层重要协议
## TCP/IP 协议族
 - TCP/IP是一个协议族,它是ARP,IP,ICMP,IGMP,UDP,TCP等多个协议的集合。
 - 传输层的TCP和UDP协议,网络层的IP协议最具有代表性。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/5.png)

## TCP - 传输控制协议
 - 用于面向连接的服务
 - 传输前需建立连接
 - 可靠的,有序的字节流传输
 - 支持流量控制与拥塞控制
 - 连接只是一种逻辑形式,它关心的是端点。
 - 与在网络上寻求一条实际的物理路径相比,这条信道更关心的是保持两个端点的联系。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/6.png)
 - 每个数据包的传递都需要对方确认,无确认就认为失败。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/7.png)

## UDP - 用户数据报协议
 - 用于无连接的服务
 - 是不可靠的数据传输
 - 无流量控制
 - 无拥塞控制
 - 源端和终端不建立连接,当传送时就简单地去抓取数据,并尽可能快地把它扔到网络上。
 - UDP传送数据的速度仅仅受应用程序生成数据的速度、计算机的能力和传输带宽的限制

## TCP 和 UDP 的应用范围
1. 使用TCP服务的应用 :
 - HTTP(WWW)
 - FTP (file transfer)
 - Telnet (remote login)
 - SMTP (email)
2. 使用UDP服务的应用 :
 - 流媒体,视频会议,因特网电话

# IP 协议
## 概念
 - IP协议的作用是将数据包从源传送到目的地。
 - 实现了在互联网络间进行寻址和分组转发 。
 - IP数据包中含有发送它的主机的地址(源地址)和接收它的主机的地址(目的地址),这些地址就是IP地址。
 - 它不提供可靠的传输服务,它不提供端到端的确认,对数据没有差错控制,不提供重发和流量控制。

## IPv4
 - 网络中每台主机都必须有一个惟一的IP地址;
 - 因特网上的IP地址具有全球唯一性;
 - IP地址是一个逻辑地址;
 - IP是层次性地址:网络号+主机号
 - 32位,4个字节,常用点分的十进制标记法:
  如 00001010 00000010 00000000 00000001记为 10.2.0.1

### IPv4 地址分类
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/8.png)

### IPv4 存在的问题
 - 地址空间使用效率较低,IPv4 资源即将耗尽
    - Internet迅速发展,越来越多的设备需要IP地址:手机、家电、...
    - IPv4地址大约有40多亿个,即将被分配完毕
    - 资源分配不均
 - IPv4对于移动特性并没有很好的支持
 - 对于某些因特网应用,需要能够对数据进行加密和鉴别,但IPv4不提供数据的加密和鉴别

## IPv6
 - 更大的地址空间,地址扩大到128bit;
 - 减少路由选择表的长度;
 - 简化协议,使路由器处理分组更迅速;
 - 提供比当前IP更好的安全性(含鉴别和保密)

### IPv6 地址格式
 - IPv6地址 = 前缀 + 接口标识
    - 前缀:相当于v4地址中的网络ID
    - 接口标识:相当于v4地址中的主机ID
 - 128位长,用冒号将128比特分割成8个16比特的部分,每个部分是一个4位的16进制数字。
 - 地址前缀长度用“/xx”来表示
 - 举例:
  3ffe:3700:1100:0001:d9e6:0b9d:14c6:45ee/64

## IP 路由选择
 - IP数据包在Internet上传递时,由路由器决定其传递路线。
 - 路由器构成了Internet的主体脉络。它们会为数据包选择一条合适的数据传递路径。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-8/9.png)

# 路由
## 路由种类
1. 静态路由
 - 由网络管理员在路由器上手工添加的路由信息
 - 不随网络拓扑变化而自动变化
 - 适合于端网络等小型网络
2. 动态路由
 - 路由器运行路由协议与邻居路由器相互动态学习得到
 - 路由信息自动更新
 - 优点:自动适应网络拓扑变化

## 各类路由的特点
1. 静态路由
 - 无论代价如何都固定路径
 - 发生故障网络即中断
2. 动态路由
 - 自动选择最低代价的路径
 - 当前路径发生故障可自动转到其他可用路径

## 典型路由算法
 - 最短路由选择算法
这种技术的主要思想是建立一个子网图,图中的每个节点代表一个路由器,每条弧线表示一条通信线路。本算法要在图中找出源节点到目标节点间的最短路径。
 - 扩散法
把收到的每一个分组信息,从除了分组到来的线路外的所有输出线路上发出。最终信息可以从源节点到达目标节点。显然扩散要产生大量的重复分组,事实上有可能是无穷多个分组,除非采用一些措施抑制这种过程。
 - 选择性扩散法
与扩散法不同的是各个节点仅向少量预先设定的邻接点发出信息。从而降低数据冗余,减少网络拥塞。

