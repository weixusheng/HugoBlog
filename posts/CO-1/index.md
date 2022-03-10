# 计算机组成原理 Part-1


计算机基本体系结构
<!--more--> 

今天开始学习组成原理啦,由于ppt很好,就直接截图啦
# 计算机系统的层次结构
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/1.png)

## 计算机体系结构和计算机组成的关系
 - 计算机体系结构:
程序员所见到的计算机系统的属性概念性的结构与功能特性（指令系统、数据类型、寻址技术、I/O机理）
 - 计算机组成:
实现计算机体系结构所体现的属性（具体指令的实现）

##  计算机的基本组成 
冯·诺依曼计算机的特点
1. 计算机由五大部件组成
2. 指令和数据以同等地位存于存储器，可按地址寻访
3. 指令和数据用二进制表示
4. 指令由操作码和地址码组成
5. 存储程序
6. 以运算器为中心
以运算器为中心
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/2.png)
以存储器为中心
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/3.png)
现代计算机的硬件框图
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/4.png)
系统复杂性管理的方法-2（3'Y）
 - 层次化（Hierachy）：将被设计的系统划分为多个模块或子模块
 - 模块化（Modularity）：有明确定义
（well-defined）的功能和接口
 - 规则性（regularity）：模块更容易被重用

## 编程举例
**计算 ax<sup>2</sup> + bx + c = (ax + b)x + c**
取x 至运算器中
乘以x 在运算器中
乘以a 在运算器中
存ax<sup>2</sup> 在存储器中
取b 至运算器中
乘以x 在运算器中
加ax<sup>2</sup> 在运算器中
加c 在运算器中
取x 至运算器中
乘以a 在运算器中
加b 在运算器中
乘以x 在运算器中
加c 在运算器中

# 指令格式
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/5.png)

## 计算 ax<sup>2</sup> + bx + c 程序清单
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/6.png)

# 存储器的基本组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/7.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/8.png)

# 运算器的基本组成及操作过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/9.png)

## 加法操作过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/10.png)

## 减法操作过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/11.png)

## 乘法操作过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/12.png)

## 除法操作过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/13.png)

# 控制器的基本组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/14.png)

## 完成指令过程
### 取数操作
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/15.png)

### 存数操作
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/16.png)

### ax<sup>2</sup> + bx + c 的运行过程
 - 将程序通过输入设备送至计算机
 - 程序首地址
 - 启动程序运行
 - 分析指令 OP(IR) CU
 - 取指令 PC-> MAR -> M -> MDR -> IR, (PC )+ 1 -> PC 
 - 执行指令 Ad(IR) -> MAR -> M -> MDR -> ACC
 - …
 - 打印结果
 - 停机

# 计算机硬件的主要技术指标
1. 机器字长:
CPU一次能处理数据的位数,与CPU中的寄存器位数有关
2. 运算速度
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/17.png)
3. 存储容量
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-1/18.png)
其中b是bit, B是byte  1B = 8b
