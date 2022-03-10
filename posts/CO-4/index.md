# 计算机组成原理 Part-4


存储器<br>
汉明码
<!--more--> 

# 只读存储器(ROM)
1. 掩模 ROM ( MROM )
行列选择线交叉处有 MOS 管为“1”
行列选择线交叉处无 MOS 管为“0”

2. PROM (一次性编程)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/1.png)

1. EPROM (多次性编程 )
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/2.png)
通过紫外线来去除其中产生的浮动栅,以此来进行数据重置

4. EEPROM (多次性编程 )
 - 电可擦写
 - 局部擦写
 - 全部擦写

5. Flash Memory (闪速型存储器)
EPROM: 价格便宜 集成度高
EEPROM: 电可擦洗重写
Flash 比 EEPROM快 具备 RAM 功能

# 存储器与 CPU 的连接
## 存储器容量的扩展
### 位扩展 (增加存储字长)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/3.png)

### 字扩展
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/4.png)
在0-9位存在第一个存储器块中,当数据达到第10位时,需要第二个存储块来进行拓展空间
此时10号地址线通过信号处理器产生对第二个存储块的信号

### 字、位扩展
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/5.png)
10,11号线为拓展信号线,通过片选译码器来判断此时应该拓展到第几个存储块

## 存储器与 CPU 的连接
 - 地址线的连接
 - 数据线的连接
 - 读/写命令线的连接
 - 片选线的连接
 - 合理选择存储芯片

![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/6.png)

### 例题
要求最小4K为系统程序区,相邻8K为用户程序区
1. 写出对应的二进制地址码
2. 确定芯片的数量及类型
3. 分配地址线
4. 确定片选信号
5. 确定片选逻辑

![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/7.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/8.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/9.png)

由ABC三个位,来确定数据存储的位置

# 存储器的校验
## 合法代码集合
1. {000,001,010,011,100,101,110,111} 检0位错、纠0位错
2. {000, 011,101,110} 检1位错,纠0位错
3. {000,111} 检1位错,纠1位错
4. {0000,1111} 检2位错,纠1位错
5. {00000,11111} 检2位错,纠2位错

## 编码的最小距离
任意两组合法代码之间 二进制位数 的 最少差异
编码的纠错 、检错能力与编码的最小距离有关
L - 1 = D + C ( D >= C )
 - L: 编码的最小距离 L = 3
 - D: 检测错误的位数 具有 一位 纠错能力
 - C: 纠正错误的位数

汉明码是具有一位纠错能力的编码

# 汉明码
## 基本思想
 - 汉明码采用奇偶检验
 - 汉明码采用分组校验

For Example：
```
我们想传输10010这串码，那么在传输的时候，就传010010，其中在开头的0就是校验位
我们想传输10000这串码，那么在传输的时候，就传110000，其中在开头的1就是校验位
两个例子的1的个数都是**偶数**(凑奇数或偶数)
```
**汉明码默认一串数据只错一位**
假设想传这一列数据1234567，先把他分个组
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/10.png)
```
P1组：1,2,3,4
P2组：2,4,5,7
P3组：3,4,6,7
```
接收方接到数据,对其中的分组数据进行奇偶判定,就可以得到出错的位,并进行纠错
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/11.png)

汉明码的组成需增添?位检测位
2<sup>k</sup> >= n + k + 1
检测位的位置
2<sup>i</sup> ( i = 0 , 1 , 2 , 3 , ... )

## 校验码的位置
汉明码的一串数据中，校验码放在2<sup>i</sup>的位置上，

## 数据分组
把表示位置的这个数，转化成二进制数。
```
第1个位置，变成第0001个位置；
第2个位置，变成第0010个位置；
第3个位置，变成第0011个位置；
第4个位置，变成第0100个位置；
第5个位置，变成第0101个位置；
第6个位置，变成第0110个位置；
```
位置符合这种形式的，XXX1，归到P1；
位置符合这种形式的，XX1X，归到P2；
位置符合这种形式的，X1XX，归到P3；
位置符合这种形式的，1XXX，归到P4；

**例如**
```
我们想传这一组码：XX1X101X011
一共11位。
标X的是校验码的位置，我们暂时不知道它的值是多少。
位置在1,3,5,7,9,11的数据进到P1组。
位置在2,3,6,7,10,11的数据进到P2组。（位置符合XX1X）
位置在4,5,6,7的数据进到P3组。（位置符合X1XX）
位置在8,9,10,11的数据进到P4组。（位置符合1XXX）
那么确定了分组，校验码的值也就顺便确定下来了。
这样整个串的码就确定下来了。
```

## 检验汉明码
1. 分组，分好P1，P2，P3……
2. 分别对每个组校验，没有错的给它0，有错的给0.
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/14.png)
**把P从大到小排列起来，得到一串1010**
For Example:
```
组别：   P5   P4   P3   P2   P1
标志：   1    0    1    0    1
```
从大到小排列起来，标志排成了一串一零串。这个数就是出错的数据的位置。
本例中，10101位置上的位错了，换成十进制是第21个位置上的数错了。

## 例题
1. 求 0101 按 “偶校验” 配置的汉明码
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/12.png)

2. 已知接收到的汉明码为 0100111
(按配偶原则配置)试问要求传送的信息是什么?
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/13.png)

# 提高访存速度的措施
 - 采用高速器件
 - 采用层次结构 Cache –主存
 - 调整主存结构

## 单体多字系统
增加存储器的带宽
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/15.png)

## 多体并行系统
### 高位交叉-顺序编址
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/16.png)

### 高位交叉-各个体并行工作
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/17.png)

### 低位交叉-各个体轮流编址
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/18.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/19.png)

### 低位交叉的特点
在不改变存取周期的前提下,增加存储器的带宽
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/21.png)

设四体低位交叉存储器,存取周期为T,总线传输周期
为 τ ,为实现流水线方式存取,应满足 T = 4 τ 
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/CO-4/22.png)

# 高性能存储芯片
1. SDRAM (同步 DRAM)
在系统时钟的控制下进行读出和写入
CPU无须等待
2. RDRAM
由Rambus开发,主要解决存储器带宽问题
3. 带 Cache 的 DRAM
在DRAM的芯片内集成了一个由 SRAM 组成的
Cache ,有利于猝发式读取

