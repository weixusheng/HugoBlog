# NNDL-反向传播


BackPropagation Algorithm
<!--more-->

# BackPropagation(反向传播)
## 视频来源
[3Blue1Brown](https://www.youtube.com/watch?v=tIeHLnjs5U8)

# 计算过程
<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP7.png" width=60%>

典型的三层神经网络的基本构成
 - Layer L1是输入层
 - Layer L2是隐含层
 - Layer L3是隐含层

我们现在手里有一堆数据{x1,x2,x3,...,xn},输出也是一堆数据{y1,y2,y3,...,yn},现在要他们在隐含层做某种变换，让你把数据灌进去后得到你期望的输出
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP1.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP2.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP3.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP4.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP5.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/BP6.jpg">

## 神经网络BP算法详解
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/1.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/2.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/3.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/4.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/5.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/6.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/7.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/8.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/9.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/10.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/11.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/12.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/13.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/14.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/15.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/16.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/17.png">

# BP详解
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/13_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/14_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/15_.png">

# 总结
[资料参考Blog](https://blog.csdn.net/woshicao11/article/details/81806512)

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/2_.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/1_.jpg">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/13_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/16_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/BP/17_.png">

