# 计算机组成原理 Part-7


DMA<br>
今天做完了答辩的PPT!准备开吹了
<!--more--> 

# DMA的特点
DMA 和程序中断两种方式的数据通路
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/1.png)
DMA 与主存交换数据的三种方式
1. 停止 CPU 访问主存
控制简单
CPU 处于不工作状态或保持状态
未充分发挥 CPU 对主存的利用率
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/2.png)
2. 周期挪用（或周期窃取）
DMA 访问主存有三种可能
 - CPU 此时不访存
 - CPU 正在访存
 - CPU 与 DMA 同时请求访存
此时 CPU 将总线控制权让给 DMA
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/3.png)

3. DMA 与 CPU 交替访问
CPU 工作周期 
 - C1 专供 DMA 访存
 - C2 专供 CPU 访存
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/4.png)
不需要 申请建立和归还 总线的使用权

# DMA 接口的功能和组成
##  DMA 接口功能
1. 向 CPU 申请 DMA 传送
2. 处理总线 控制权的转交
3. 管理 系统总线、控制数据传送
4. 确定 数据传送的 首地址和长度
修正传送过程中的数据地址和长度
5. DMA 传送结束时，给出操作完成信号

##  DMA 接口组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/5.png)

# DMA 的工作过程
## DMA 传送过程
 - 预处理
 - 数据传送
 - 后处理

## 预处理
通过几条输入输出指令预置如下信息
 - 通知 DMA 控制逻辑传送方向（入/出）
 - 设备地址 DMA 的 DAR
 - 主存地址 DMA 的 AR
 - 传送字数 DMA 的 WC

##  DMA 传送过程示意
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/6.png)

## 数据传送过程（输出）
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/7.png)

## 后处理
校验送入主存的数是否正确
是否继续用 DMA
测试传送过程是否正确，错则转诊断程序
由中断服务程序完成

## DMA 接口与系统的连接方式
### 具有公共请求线的 DMA 请求
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/8.png)

### 独立的 DMA 请求
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/9.png)

## DMA 方式与程序中断方式的比较
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/10.png)

# DMA 接口的类型
## 选择型
在物理上连接多个设备
在逻辑上只允许连接一个设备
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/11.png)

## 多路型
在物理上连接多个设备
在逻辑上允许连接多个设备同时工作
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/12.png)

### 多路型 DMA 接口的工作原理
传输速度更快的设备,其优先级越高
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-7/13.png)





