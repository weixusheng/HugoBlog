# 物联网概论 PART-4



物联网标识技术
<!--more-->

物联网标识技术
# 条形码
 - 1920, Kermode发明了最早的条码
 他的想法是在信封上做条形码标记,条形码中的信息是收信人的地址,就象今天的邮政编码。为此Kermode发明了最早的条形码标识,设计方案非常的简单,即一个“条”表示数字“1”,二个“条”表示数字“2”,以次类推。

 - 公牛眼
20世纪40年代,称作“公牛眼”条码。伍德兰德和西尔沃提出
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/1.png)

 - 1959年申请专利
伍德兰德和费伊赛尔申请专利,0~9用7段平行条构成

# 一维码
## 概念
1. 条码(Bar Code)
将宽度不等的多个黑条和空白,按照一定的编码规则排列,用以表达一组信息的图形标识符。
2. 码制
条形码的编码要求,数字系统的主要功能是处理信息。 因此必须将信息表示成能够识别的电路,便于运算存储的形式。
3. 条码字符集
某种码制所表示的全部字符的集合
4. 非连续性
非连续性是指每个条码字符之间存在间隔
5. 定长条码与非定长条码
定长条码是指仅能表示固定字符个数的条码。如:EAN-13条码
非定长条码是指能表示可变字符个数的条码。如:EAN-128条码。
6. 双向可读性
7. 自校验性

## 结构
一个完整的条码的组成次序依次为:静区(前)、起始符、数据符、(中间分隔符,主要用于EAN码)、校验符、终止符、静区(后)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/3.png)

## 一维条码的识读原理
电信号输出到条码扫描器的放大电路增强信号之后,再送到整形电路将模拟信号转换成数字信号。白条、黑条的宽度不同,相应的电信号持续时间长短也不同。然后译码器通过测量脉冲数字电信号0,1的数目来判别条和空的数目。通过测量0,1信号持续的时间来判别条和空的宽度。最后,由计算机系统进行数据处理与管理,物品的详细信息便被识别了。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/4.png)

# 二维码
## 二维码的组成
二维条码是用某种特定的几何图形按一定规律相应元素位置上用“点”表示二进制“1”, 用“空”表示二进制“0”,由“点”和“空”的排列组成的代码。

## 二维码与一维码比较
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/5.png)

## 二维码的优点
1. 可靠性强
二维码的读取准确率远远超过人工记录,平均每15000个字符才会出现一个错误。
2. 效率高
二维码的读取速度很快,每秒可读取40个字符。
3. 成本低
与其它自动化识别技术相比较,二维码技术仅仅需要一小张贴纸和相对构造简单的光学扫描仪,成本相当低廉。
4. 易于制作
条形码制作:条形码的编写很简单,制作也仅仅需要印刷
5. 灵活实用
条形码符号可以手工键盘输入,也可以和有关设备组成识别系统实现自动化识别,还可和其他控制设备联系起来实现整个系统的自动化管理。
6. 高密度
二维条码通过利用垂直方向的堆积来提高条码的信息密度,而且采用高密度图形表示,因此不需事先建立数据库,真正实现了用条码对信息的直接描述。
7. 纠错功能
二维条形码不仅能防止错误,而且能纠正错误,即使条形码部分损坏,也能将正确的信息还原出来。

## 二维码的分类
1. 线性堆叠式二维码 是在一维条码编码原理的基础上,将多个一维码在纵向堆叠而产生的。典型的码制如: Code 16K、Code 49、PDF417等。

2. 矩阵式二维码 是在一个矩形空间通过黑、白像素在矩阵中的不同分布进行编码。典型的码制如: Aztec、Maxi Code、QR Code、 Data Matrix等。

# QR-CODE
## 定义
QR码是由日本电装(Denso)公司于1994年9月研制的一种矩阵二维码符号,QR码除具有一维条码及其它二维条码所具有的信息容量大、可靠性高、可表示汉字及图象多种文字信息、保密防伪性强等优点。普通的一维条码只能在横向位置表示大约20位的字母或数字信息,无纠错功能,使用时候需要后台数据库的支持,而QR码二维条码是横向纵向都存有信息,可以放入字母、数字、汉字、照片、指纹等大量信息,相当一个可移动的数据库。

## 优点
 - QR码比其他二维码相比,具有识读速度快、数据密度大、占用空间小的优势。QR码的三个角上有三个寻象图形,使用CCD识读设备来探测码的位置、大小、倾斜角度、并加以解码,实现360度高速识读。

 - 每秒可以识读30个含有100个字符QR码。QR码容量密度大,可以放入2108个汉字、7089个数字、4296个英文字母。QR码用数据压缩方式表示汉字,仅用13bit即可表示一个汉字,比其他二维条码表示汉字的效率提高了20%。

 - QR具有4个等级的纠错功能,即使破损或破损也能够正确识读。QR码抗弯曲的性能强,即使将QR码贴在弯曲的物品上也能够快速识读。

## QR-CODE编码原理
每个QR码符号由名义上的正方形模块构成,组成一个正方形阵列,它由编码区域和包括**寻象图形、分隔符、定位图形和校正图形在内的功能图形**组成。功能图形不能用于数据编码。符号的四周由空白区包围。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/6.png)

## 区域组成
1. 寻象图形
寻象图形包括三个相同的位置探测图形,分别位于符号的左上角、右上角和左下角,如图2所示。每个位置探测图形可以看作是由3个重叠的同心的正方形组成,它们分别为7x7个深色色模块、5x5个浅模块和3x3个深色模块。如下图所示,位置探测图形的模块宽度比为1:1:3:1:1。符号中其他地方遇到类似图形的可能性极小,因此可以在视场中迅速地识别可能的QR码符号。识别组成的寻象图形的三个位置探测图形,可以明确地确定视场中符号的位置和方向。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/7.png)

2. 分隔符
在每个位置探测图形和编码区域之间有宽度为1个模块的分隔符,它全部由浅色模块组成。

3. 定位图形
水平和垂直定位图形分别为一个模块宽的一行和一列,由深色浅色模块交替组成,其开始和结尾都是深色模块。水平定位图形位于上部的两个位置探测图形之间,符号的第7行。垂直定位图形位于左侧的两个位置探测图形之间,符号的第7列。它们的作用是确定符号的密度和版本,提供决定模块坐标的基准位置。

4. 校正图形
每个校正图形可看作是3个重叠的同心正方形,由5×5个的深色模块,3×3个的浅色模块以及位于中心的一个深色模块组成。校正图形的数量视符号的版本号而定,在模式2的符号中,版本2以上(含版本2)的符号均有校正图形。

5. 编码区域
编码区域包括表示数据码字、纠错码字、版本信息和格式信息的符号字符。

6. 空白区
空白区为环绕在符号四周的4个模块宽的区域,其反射率应与浅色模块相同。

## QR-CODE编码过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/8.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/9.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/10.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/11.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/12.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/13.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/14.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/15.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/16.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/17.png)

# 射频技术
## 定义
RFID是射频识别技术(Radio Frequency Identification)的英文缩写,利用 射频信号 通过 空间耦合 (交变磁场或电磁场)实现 无接触信息传递 并通过所传递的信息达到 识别 目的。它是上世纪90年代兴起的自动识别技术,首先在欧洲市场上得以使用,随后在世界范围内普及。RFID较其它技术明显的 优点 是电子标签和阅读器 无需接触 便可完成识别。射频识别技术改变了条形码依靠"有形"的一维或二维几何图案来提供信息的方式, 通过芯片 来 提供存储 在其中的数量巨大的"无形"信息。
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/32.png)

## 历史发展
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/2.png)

## 工作原理
 - 基本组成: 工业界经常将RFID系统分为标签,阅读器和天线三大组件。
 - 工作原理: 阅读器通过天线发送电子信号,标签接收到信号后发射内部存储的标识信息,阅读器再通过天线接收并识别标签发回的信息,最后阅读器再将识别结果发送给主机。

# 阅读器
1. 供能
阅读器为射频标签工作提供能量
2. 安全保护
阅读器的通信安全性保证功能,如使用加密、解密技术
3. 通信
阅读器和射频标签之间的通信阅读器和应用层(中间件)之间的通信
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/18.png)

## 分类
按工作频率
RFID系统的工作原理与其所使用的射频信号频率有关。工作频率越高,识别距离越远,数据传输速率越高,信号衰减越厉害,对障碍物越敏感。

1. 低频和高频阅读器
工作频率一般小于1m,典型工作频率125kHz、135kHz、6.78MHz、13.56MHz和27.125MHz
2. 超高频和特高频阅读器
工作频率一般大于1m,典型工作频率有433MHz、860MHz~960MHz、2.45GHz和5.8GHz

## 信号处理与控制模块
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/19.png)

## 射频模块
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/20.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/21.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/22.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/23.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/24.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/25.png)

# 标签
## 标签功能
1. 存储数据
标签内存储和物品相关的信息,如标识符、生产日期、生产厂家等。
2. 能量获取
标签可以从阅读器发射的电磁场中吸收能量,为标签自生供电。
3. 非触碰式读写
标签可以在距离阅读器一定距离的范围内被识别。

## 标签分类
### 能量来源
 - 有源标签: 主动标签,依靠自身电池。
 - 无源标签: 依靠反射阅读器发射的载波信号来获取能量
 - 半无源标签: 电路板上集成电池,但作为辅助备用

### 工作频率
 - 低频标签:
   - 30kHz~300kHz
   - 一般为无源
   - 电感耦合
   - 穿透力强
 - 高频标签
   - 300kHz~30MHz
   - 一般为无源
   - 电感耦合
   - 传输快、存储大
 - 超高频标签
   - 0.3GHz~3GHz
   - 无源或有源
   - 电磁反向散射耦合
   - 距离远、速率高、移动场景、多标签读写性能好

## 标签组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/26.png)

### 天线
决定了标签的尺寸接受阅读器发射的射频信号将芯片数据发送给阅读器对于无源标签,天线还用来供能
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/27.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/28.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/29.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/30.png)

### 芯片
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-4/31.png)
