# CMU213_Part-1


原码,反码,补码,有符号数,无符号数
<!--more--> 

### 1. 原码,反码,补码
一开始讲到机器原码反码和补码,有点懵,野路子学计算机导致知识点不细致,看了很多视频和博客算是明白了

[这篇](https://blog.csdn.net/sd631032049/article/details/72781138)算是讲的最好的了

1. 原码作为人能识别的数字,给机器运算的话,会因为符号位的识别,浪费很多资源,所以想到将符号位也加入到二进制的运算中.最后只使用加法器

2. 一开始使用反码运算,取反运算正是电路擅长的但是由于+0和-0,造成两个0的情况,又想出了补码运算

3. 运算完成之后,将进位去除,还原为原码.即最后的结果

4. 所以目前机器内存中存储的数据都是补码存储,在单片机中输出数据很多也是补码

[一篇讲补码很好的文章](https://www.cnblogs.com/effulgent/archive/2011/10/30/two_s_complement.html)

### 2.signed和unsigned类型的运算
第一节课的后部分讲到signed和unsigned进行算数比较以及两者的范围,这里着重记录比较
这里有两个重点需要记住

 - 有符号数和无符号数混合运算时（包括逻辑运算和算术运算）默认会将有符号数看成无符号数进行运算，返回无符号数结果

 - 32位机器中，比int小的整型（包括short 、unsigned short 、 unsigned char和char）在运算中都要转换成int然后进行运算

**这两篇博客解释的很好**

[第一篇](https://blog.csdn.net/happygaohualei/article/details/86078182)

 [第二篇](https://www.cnblogs.com/qingergege/p/7507533.html)

