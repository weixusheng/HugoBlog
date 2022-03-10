# OS总结 Part-2



绪论,进程
<!--more-->

# 绪论
## 发展历史
1. 操作系统发展的四个阶段
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/1.png)

2. 计算机硬件的重大进展
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/2.png)

3. Unix的特点
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/3.png)

## 操作系统的结构
### 整体式结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/4.png)
特点:
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/5.png)

### 层次式结构
计算机网络中的典型分层结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/6.png)
操作系统分层结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/7.png)
特点:
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/8.png)

### 微内核结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/9.png)

# 态的转换
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/10.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/11.png)

# 存储体系
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/12.png)

# 中断
## 定义
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/13.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/14.png)

## 中断类型
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/15.png)

## 中断相应过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/16.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/17.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/18.png)

# 进程
## 进程特征
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/19.png)

## 进程状态
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/20.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/21.png)

## Linux进程状态及标识
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/22.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/24.png)

## 进程PCB
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/23.png)

## 进程撤销
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/25.png)

## Windows进程创建
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/26.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/27.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/28.png)

## Linux进程创建
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/29.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/30.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/31.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/32.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/33.png)

## 创建线程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/33.png)

# 同步互斥
## 临界资源
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/34.png)

## 同步
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/35.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/36.png)

## 信号灯机制
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/37.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/38.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/39.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/40.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/41.png)

## Windows同步机制
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/42.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/43.png)

## 互斥量
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/44.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/45.png)

## Linux进程实现同步
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/46.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/47.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/48.png)

## 进程通信-管道通信
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/49.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/50.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/51.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/52.png)

## Linux终端上的信号机制
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/53.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/54.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/55.png)

## 编程实现进程之间的信号通信
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/56.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/57.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/58.png)

## 哲学家进餐-同步
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/59.png)

## Linux进程
### 进程类型
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/60.png)

### 进程优先级及调度策略
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/61.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/62.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/63.png)

当linux中的进程创建子进程时，子进程时间继承自父进程的一半，且当前父进程时间减半
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/OS-2/64.png)
