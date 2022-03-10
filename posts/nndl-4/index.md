# NNDL-向量化


数据的向量化处理以及Python Broadcasting
<!--more-->

# 向量化处理的原因
[为什么向量化会更快](https://www.zhihu.com/question/67652386)
摘自一个回答:
Numpy运算是向量化的，进行是CPU指令集层面的并行运算，for循环是串行运算，效率肯定差多了。想象一下，一个大矩阵做运算，for循环是一次提取两个值，放入内存运算后返回结果，再提取2个再运算返回结果，如果循环。Numpy是一次性把所有值都提入内存同时运算同时返回结果，效率肯定不一样。向量化的优势是快，劣势是费内存，矩阵太大就放不进去了。for循环一次2个值多大都能算，就是太慢没法实用。科学计算都是向量化的，for循环只用于工业界软件开发等数据不大但对运行环境有资源限制的地方。

# 向量化(Vectorization)
向量化是非常基础的去除代码中for循环的艺术，在深度学习安全领域、深度学习实践中，你会经常发现自己训练大数据集，因为深度学习算法处理大数据集效果很棒，所以你的代码运行速度非常重要，否则如果在大数据集上，你的代码可能花费很长时间去运行，你将要等待非常长的时间去得到结果。所以在深度学习领域，运行向量化是一个关键的技巧，让我们举个栗子说明什么是向量化。
在逻辑回归中你需要去计算$z = s^Tx+b, w,x$都是列向量,如果你有很多的特征那么就会有一个非常大的向量,所以如果你想使用非向量化去计算$w^Tx$你需要用python
```c++
​x
z=0
for i in range(n_x)
    z+=w[i]*x[i]
z+=b
```
这是一个非向量化的实现，你会发现这真的很慢，作为一个对比，向量化实现将会非常直接计算w^Tx，代码如下：
```python
z=np.dot(w,x)+b
```
![](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/5/1.png)

```python
import numpy as np #导入numpy库
a = np.array([1,2,3,4]) #创建一个数据a
print(a)
# [1 2 3 4]
import time #导入时间库
a = np.random.rand(1000000)
b = np.random.rand(1000000) #通过round随机得到两个一百万维度的数组
tic = time.time() #现在测量一下当前时间

#向量化的版本
c = np.dot(a,b)
toc = time.time()
print(“Vectorized version:” + str(1000*(toc-tic)) +”ms”) #打印一下向量化的版本的时间

#继续增加非向量化的版本
c = 0
tic = time.time()
for i in range(1000000):
    c += a[i]*b[i]
toc = time.time()
print(c)
print(“For loop:” + str(1000*(toc-tic)) + “ms”)#打印for循环的版本的时间
```
![](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/5/2.png)

在两个方法中，向量化和非向量化计算了相同的值，如你所见，向量化版本花费了1.5毫秒，非向量化版本的for循环花费了大约几乎500毫秒，非向量化版本多花费了300倍时间。所以在这个例子中，仅仅是向量化你的代码，就会运行300倍快。这意味着如果向量化方法需要花费一分钟去运行的数据，for循环将会花费5个小时去运行。

一句话总结，以上都是再说和for循环相比，向量化可以快速得到结果。

你可能听过很多类似如下的话，“大规模的深度学习使用了GPU或者图像处理单元实现”，但是我做的所有的案例都是在jupyter notebook上面实现，这里只有CPU，CPU和GPU都有并行化的指令，他们有时候会叫做**SIMD指令**，这个代表了一个单独指令多维数据，这个的基础意义是，如果你使用了built-in函数,像np.function或者并不要求你实现循环的函数，它可以让**python的充分利用并行化计算**，这是事实在GPU和CPU上面计算，GPU更加擅长SIMD计算，但是CPU事实上也不是太差，可能没有GPU那么擅长吧。接下来的视频中，你将看到向量化怎么能够加速你的代码，经验法则是，**无论什么时候，避免使用明确的for循环**。

# 向量乘法
## 向量的内积(点乘)
### 定义
概括地说，向量的内积（点乘/数量积）。对两个向量执行点乘运算，就是对这两个向量对应位一一相乘之后求和的操作，如下所示，对于向量a和向量b：
$a = [a_1,a_2,a_3...a_n] b = [b_1,b_2,b_3...b_n]$
因此a和b的点积为
$a·b = a_1b_1+a_2b_2+a_3b_3...+a_nb_n$
这里要求一维向量a和向量b的行列数相同。注意：点乘的结果是一个**标量**(数量而不是向量)
定义：两个向量a与b的内积为 **a·b = |a||b|cos∠(a, b)**，特别地，0·a =a·0 = 0；若a，b是非零向量，则a与b****正交的充要条件是a·b = 0。

### 向量内积的性质：
 - a^2 ≥ 0；当a^2 = 0时，必有a = 0. （正定性）
 - a·b = b·a. （对称性）
 - (λa + μb)·c = λa·c + μb·c，对任意实数λ, μ成立. （线性）
 - cos∠(a,b) =a·b/(|a||b|).
 - |a·b| ≤ |a||b|，等号只在a与b共线时成立.

### 向量内积的几何意义
内积（点乘）的几何意义包括：
 - 表征或计算两个向量之间的夹角
 - b向量在a向量方向上的投影

## 外积（叉乘）
概括地说，两个向量的外积，又叫叉乘、叉积向量积，其运算结果是一个向量而不是一个标量。并且两个向量的外积与这两个向量组成的坐标平面垂直。

### 定义
向量a与b的外积a×b是一个向量，其长度等于|a×b| = |a||b|sin∠(a,b)，其方向正交于a与b。并且，(a,b,a×b)构成右手系。
特别地，0×a = a×0 = 0.此外，对任意向量a，a×a=0。
$a = (x_1,y_1,z_1)$
$b = (x_2,y_2,z_2)$
a和b的外积公式为
$a\times b = \left| \begin{matrix}
    i & j & k \\
    x_1 & y_1 & z_1 \\
    x_2 & y_2 & z_2
\end{matrix} \right|$

其中i = (1,0,0) j = (0,1,0) k= (0,0,1)
根据i,j,k之间的关系可以得出
$a\times b = (y_1z_2-y_2z_1, -(x_1z_2-x_2z_1), x_1y_2-x_2y_1)$

### 向量外积的性质
 - a × b = -b × a. （反称性）
 - (λa + μb) × c = λ(a ×c) + μ(b ×c). （线性）

### 向量外积的几何意义
在三维几何中，向量a和向量b的外积结果是一个向量，有个更通俗易懂的叫法是**法向量**，该向量垂直于a和b向量构成的平面。

# 向量化逻辑回归
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/01_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/02_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/03_.png">

# 向量化 logistic 回归的梯度输出
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/04_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/05_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/06_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/07_.png">

# Broadcasting in Python
Python中的矩阵计算便捷方法
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/08_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/09_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/10_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/11_.png">

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/NNDL/4/12_.png">

