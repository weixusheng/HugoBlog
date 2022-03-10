# 计算机网络 Part-2


物理层
<!--more-->

# 物理层下面的传输媒体
双绞线
 - 屏蔽双绞线 STP
 - 无屏蔽双绞线 UTP

同轴电缆
 - 50 同轴电缆 用于数字传输，由于多用于基带传输，也叫基带同轴电缆；
 - 75 同轴电缆用于模拟传输，即宽带同轴电缆。

![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/1.png)

## 网线
### 直通线
直通线应用最广泛，这种类型的以太网电缆用来实现下列连接:
 - 主机到交换机或集线器
 - 路由器到交换机或集线器 


### 交叉线
应用:
 - 交换机到交换机
 - 集线器到集线器
 - 主机到主机
 - 集线器到交换机
 - 路由器直连到主机

![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/2.png)

## 光纤
### 折射
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/3.png)

### 工作原理
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/4.png)

### 单模光纤与多模光纤
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/5.png)

## 电磁波的频谱
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/6.png)

## 物理层设备----集线器
工作特点：它在网络中只起到**信号放大和重发**作用，其目的是扩大网络的传输范围，而**不具备**信号的定向传送能力
最大传输距离：100m
集线器是一个大的冲突域

# 信道复用技术
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/7.png)

## 频分复用(FDM)
Frequency Division Multiplexing
用户在分配到一定的频带后，在通信过程中自始至终都占用这个频带
频分复用的所有用户在**同样的时间占用不同的带宽资源**（请注意，这里的“带宽”是频率带宽而不是数据的发送速率）
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/8.png)
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/9.png)
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/10.png)
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/11.png)
由于频率范围极为广泛,因此可进行多组整合
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/12.png)

## 时分复用(TDM)
Time Division Multiplexing
时分复用则是将时间划分为一段段等长的时分复用帧（TDM 帧）。每一个时分复用的用户在每一个 TDM 帧中占用固定序号的时隙
每一个用户所占用的时隙是周期性地出现（其周期就是 TDM  帧的长度对应的时间）
TDM 信号也称为等时(isochronous)信号。
时分复用的所有用户是在不同的时间占用同样的频带宽度。
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/13.png)
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/14.png)
多组数据进行重组
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/15.png)
在生成和解析过程中,使用了类似轮拨盘的算法
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/16.png)
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/17.png)
为了防止资源上的浪费,可进行不固定性的拼凑帧操作
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/18.png)

## 波分复用(WDM)
Wavelength Division Multiplexing
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/19.png)
![IMG](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CN-2/20.png)



