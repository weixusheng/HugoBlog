# CMU213_Part-2


二进制运算
<!--more--> 

### 1.位宽不等的二进制运算
```
符号位扩展，补齐后运算即可。
例如：-1的4位补码为 1111；+1的8位补码为000000001。显然，两个数相加的结果为零
计算过程如下：
```
<div align=center>
	<img src="https://iknow-pic.cdn.bcebos.com/80cb39dbb6fd5266bbcd61ada218972bd50736a0?x-bce-process=image/resize,m_lfit,w_600,h_800,limit_1" width="">
</div>

### 2.二进制运算溢出

 - **溢出的概念**

&emsp; 对一个N位二进制补码，其可以表达的范围是 - 2N ~ 2N-1之间。如果超出这个范围就称为溢出了。

&emsp; 拿上面的-3-6来说，我们刚刚在计算时，转换为二进制补码是4位的。它的取值范围是-8 ~ +7之间。而我们想要的结果是-9,比范围的最小值还要小，这个叫做负溢出。同理如果想要的结果比最大值还要大，那么就叫做正溢出，如取值范围是-8 ~ +7之间，想要的结果是+8，那么就是正溢出。

 - **溢出后该如何正确计算结果**


　　解决方式: 将位宽扩大一位，按前面的判定方法进行判定。

　　举例: -3-6  =  -9  前面这是一个负溢出，我们在转换为二进制时进行位宽扩大，以提升取值范围。

　　此处 -3 二进制写成 10011(5位比开始多一位), -6二进制写成10110(5位比开始多一位)。再进行补码运算，10011--> 11100+1-->11101, 10110-->11001+1-->11010.
```
      11101
+   11010

= 110111
```
最高位舍去,得到 10111,取反 01000, +1 = 01001= -9

[参考博客地址](https://www.cnblogs.com/Jamesjiang/p/8947252.html)

CSAPP这门课知识点很多,看书之后再来添加知识点
