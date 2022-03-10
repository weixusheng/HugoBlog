# 计算机组成原理 Part-5


cache<br>
辅助存储器
<!--more--> 

# 高速缓冲存储器
避免 CPU “空等” 现象,程序访问的局部性原理
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/1.png)

## Cache 的工作原理
### 主存和缓存的编址
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/2.png)
主存和缓存都是按块存储,并且块内地址相同

### 命中与未命中
缓存共有 C 块
主存共有 M 块 M >> C
 - 命中
主存块调入缓存
主存块与缓存块建立了对应关系
用标记记录与某缓存块建立了对应关系的主存块号

 - 未命中
主存块未调入缓存
主存块与缓存块未建立对应关系

### Cache 的命中率
CPU 欲访问的信息在 Cache 中的 比率
**命中率 与 Cache 的 容量 与 块长 有关**
一般每块可取 4 ~ 8 个字
块长取一个存取周期内从主存调出的信息长度

### Cache –主存系统的效率
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/3.png)

## Cache 的基本结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/4.png)

## Cache 的读写操作
### 读操作
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/5.png)

### 写操作
在写的过程中,需要保持Cache和主存的一致性,因此有两种方法
 - 写直达法(Write – through)
写操作时数据既写入Cache又写入主存
写操作时间就是访问主存的时间
Cache块退出时,不需要对主存执行写操作,更新策略比较容易实现

 - 写回法(Write – back)
写操作时只把数据写入 Cache 而不写入主存
当 Cache 数据被替换出去时才写回主存
写操作时间就是访问 Cache 的时间
Cache块退出时,被替换的块需写回主存,增加了Cache的复杂性

## Cache 的改进
 - 增加 Cache 的级数
片载(片内)Cache
片外 Cache
目前的CPU一般有三层cache,单核内部 -> 多核共用cache -> cpu整体共用cache

 - 统一缓存和分立缓存
指令 Cache
数据 Cache
与指令执行的控制方式有关

## Cache – 主存的地址映射
### 直接映射
img
每个块区的固定位置 -> Cache中的固定位置
**每个缓存块 i 可以和若干个主存块对应**
**每个主存块 j 只能和一个缓存块对应**

### 全相联映射
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/6.png)
**主存中的任一块可以映射到缓存中的任一块**

### 组相联映射
两者的巧妙结合
每个cache块有多个列
每个cache块对应内存中的多个位置,但是这多个位置可以自由的存入对应cash块的任意一列,所以将其这些cache块看作是多个组
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/7.png)

# 辅助存储器
1. 特点
不直接与 CPU 交换信息
2. 磁表面存储器的技术指标
    - 记录密度:道密度:D<sub>t</sub>,位密度:D<sub>b</sub>
    - 存储容量 C = n × k × s
    - 平均寻址时间: 寻道时间 + 等待时间
    - 辅存的速度
        - 寻址时间
        - 磁头读写时间
    - 数据传输率 D<sub>r<sub> = D<sub>b<sub> × V
    - 误码率:出错信息位数与读出信息的总位数之比

## 磁记录原理和记录方式
### 磁记录原理-写
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/8.png)

### 磁记录原理-读
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/9.png)

## 硬磁盘存储器
### 硬磁盘存储器的类型
 - 固定磁头和移动磁头
 - 可换盘和固定盘

### 硬磁盘存储器结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/10.png)

## 磁盘驱动器
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-5/11.png)

### 磁盘控制器
 - 接收主机发来的命令,转换成磁盘驱动器的控制命令
 - 实现主机和驱动器之间的数据格式转换
 - 控制磁盘驱动器读写

磁盘控制器是主机与磁盘驱动器之间的接口

### 盘片
由硬质铝合金材料制成

## 光盘存储器
### 概述
采用光存储技术
利用激光写入和读出
 - 第一代光存储技术-采用非磁性介质-不可擦写
 - 第二代光存储技术-采用磁性介质-可擦写
### 光盘的存储原理
只读型和只写一次型: 热作用(物理或化学变化)
可擦写光盘: 热磁效应



