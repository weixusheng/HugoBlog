# 计算机组成原理 Part-6


今天论文交到院里,查重过啦<br>
读书笔记还差10页没写,答辩PPT明天做...吼!<br>
计组今天开始了I/O的学习
<!--more-->

# I/O 概述
## 输入输出系统的发展概况
1. 早期
分散连接: CPU和I/O设备串行工作程序查询方式
2. 接口模块和 DMA 阶段
总线连接: CPU和I/O设备并行工作
3. 具有通道结构的阶段
4. 具有 I/O 处理机的阶段

## 输入输出系统的组成
1. I/O 软件
    - I/O 指令: CPU指令的一部分
    - 通道指令: 通道自身的指令
        指出数组的首地址、传送字数、操作命令
        如 IBM/370 通道指令为 64 位

2. I/O 硬件
    - I/O接口
    - 设备控制器
    - 通道

## I/O 设备与主机的联系方式
1. I/O 设备编址方式
    - 统一编址: 用取数、存数指令
    - 不统一编址: 有专门的 I/O 指令

2. 设备选址: 用设备选择电路识别是否被选中
3. 传送方式
    - 串行
    - 并行

## 联络方式
1. 立即响应
2. 异步工作采用应答信号
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/1.png)
3. 同步工作采用同步时标

## I/O 设备与主机的连接方式
1. 辐射式连接
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/2.png)
2. 总线连接
便于增删设备

## I/O设备与主机信息传送的控制方式
### 程序查询方式
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/3.png)

### 程序中断方式
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/4.png)
程序中断方式流程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/5.png)

### DMA 方式
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/6.png)

### 三种方式的 CPU 工作效率比较
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/7.png)

# I/O接口
## 接口的功能和组成
### 总线连接方式的 I/O 接口电路
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/8.png)

### 接口的功能
1. 选址功能: 设备选择电路
2. 传送命令的功能: 命令寄存器、命令译码器
3. 传送数据的功能: 数据缓冲寄存器
4. 反映设备状态的功能: 设备状态标记
    - 完成触发器 D
    - 工作触发器 B (设备是否进入工作状态)
    - 中断请求触发器 INTR
    - 屏蔽触发器 MASK

### I/O 接口的基本组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/9.png)

# 程序查询方式
## 程序查询流程-单个设备
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/10.png)

## 程序查询流程-多个设备
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/11.png)

## 程序流程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/12.png)

## 程序查询方式的接口电路
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/13.png)

# 程序中断方式
## 中断的概念
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/14.png)
过程中部分并行工作
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/15.png)

## 程序中断方式的接口电路
### 求触发器和中断屏蔽触发器
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/16.png)

### 排队器
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/17.png)
设备 1<sub>#</sub>、2<sub>#</sub>、3<sub>#</sub>、4<sub>#</sub>
**优先级按降序排列**
INTRi = 1 有请求 即 INTRi的非 = 0

### 中断向量地址形成部件
由硬件产生向量地址
再由向量地址找到入口地址
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/18.png)


### 程序中断方式接口电路的基本组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/19.png)

## I/O 中断处理过程
1. 条件
允许中断触发器 EINT = 1
用 开中断 指令将 EINT 置 “1”
用 关中断 指令将 EINT 置“ 0” 或硬件 自动复位
2. 时间
当 D = 1（随机）且 MASK = 0 时
在每条指令执行阶段的结束前
CPU 发 中断查询信号（将 INTR 置“1”）
以输入为例:
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/20.png)

## 中断服务程序流程
1. 中断服务程序的流程
 - 保护现场
    - 程序断点的保护: 中断隐指令完成
    - 寄存器内容的保护: 进栈指令
 - 中断服务
 对不同的 I/O 设备具有不同内容的设备服务
 - 恢复现场: 出栈指令
 - 中断返回: 中断返回指令

2. 单重中断和多重中断
单重中断: 不允许中断 现行的 中断服务程序
多重中断: 允许级别更高的中断源,中断现行的中断服务程序
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/21.png)

## 主程序和服务程序抢占 CPU 示意图
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-6/22.png)


