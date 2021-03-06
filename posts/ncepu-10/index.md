# NCEPU 数据结构简答题



数据结构
<!--more-->

## 请简述数据结构的定义和组成:
数据结构是相互之间存在的一种或多种特定关系的数据元素的集合,数据结构包括三个方面:逻辑结构,存储结构,数据的运算

## 请简述算法的五个重要的特性
1. 有穷性: 一个算法必须总是在执行有穷步后结束,并且每一步都可在有穷时间内完成
2. 确定性: 算法中的每一趟都必须有确切的含义,即相同的输出只能得到相同的结果
3. 可行性: 一个算法是可行的
4. 输入: 一个算法有0个或多个输出
5. 输入: 一个算法有0个或多个输入

## 简述好的一个算法应该达到那些目标
1. 正确性: 算法应当能够正确解决所求问题
2. 可读性: 算法应当具有良好的可读性
3. 健壮性: 当输入非法数据时,算法也能及时的做出反应或处理,不会产生莫名其妙的输出结果
4. 效率和低存储需求: 效率是算法执行的时间,存储量需求是算法执行过程中所需要的最大存储空间

## 请简述线性表和顺序表的区别
线性表是逻辑结构,表示元素之间的一对一关系,顺序表是存储结构,指的是所有的数据在存储空间上顺序排序列,而跟具体的操作方式无关

## 头结点和头指针的区分
头指针是指链表中指向第一个结点的指针,若链表有头结点,则是指向头结点的指针
头结点是为了操作的统一和方便设立的,放在第一个元素的结点之前,一般数据域无意义

## 简述链表引入头结点带来的优点
1. 第一个位置的插入和删除更加方便
2. 同意了空表和非空表的处理,若使用头结点,无论表是否为空,头指针都指向头结点

## 简述头插法和尾插法的区别
头插法:不断将新节点插入表头后面,头插法会改变数据的输入顺序
尾插法:新来的节点插入链表的末尾处,数据的顺序不改变

## 简述顺序表和链表存储的优缺点
顺序表存储: 优点: 存取速度更快,可以实现随机存取,数据存取密度较高. 缺点: 删除和插入较慢,不可以增加长度
链表存储: 优点: 插入和删除的速度快,并且不会发生存储溢出的问题. 缺点: 查找速度过慢,不能实现随机存取

## 字符串和字符数组的区别
字符串指针变量本身是一个变量,用于存放存放字符串的首地址,而字符串本身是存在以该首地址为首的一块连续内存空间并以'\0'作为串的结束,字符数组是由于若干个数组元素组成的,它可以用来存放整个字符串

## 数组和线性表的关系
数组是线性表的推广,一维数组可以看作是一个线性表,二维数组可以看作是元素是线性表的线性表,数组一旦被定义,他的维数和维界就不会发生改变,因此除了结构初始化和销毁之外,数组只会有存取元素和修改元素的操作

## 请叙述完全二叉树的定义
对于深度为K的,有n个结点的二叉树,当且仅当其每一个结点都与深度为K的满二叉树中编号从1到n的结点一一对应,则称之为完全二叉树

## 简述极大连通子图和极小连通子图
极大连通子图是无向图的连通分量,极大即要求连通子图中包含其所有的边,极小连通子图既保持图连通又要使得边数最小的子图

## 算法的稳定性
假定在待排序的记录序列中,存在多个具有相同关键字的记录,若经过排序后这些记录的相对次序保持不变,则称算法是稳定的

## 什么是抽象数据类型
ADT是指一个数学模型及定义在该模型上的一组操作,抽象数据类型的定义仅取决于他的逻辑特性,而与其在计算机内部如何表示和实现无关

## 数据的基本单位和最小单位分别是什么
基本单位: 数据元素
最小单位: 数据项

## 什么是数据结构,包含哪些方面的内容
数据结构是相互之间有一种或多种特定关系的数据元素的集合
数据结构包括三个方面的内容: 逻辑结构,存储结构,数据的运算

## 数据的逻辑结构
集合 线性结构 树形结构 图状结构或网状结构

## 数据的存储结构
顺序存储 链式存储 索引存储 散列存储

## 算法的特性有哪些
有穷性 确定性 输入 输出 可行性

## 好的算法的要求
健壮性 正确性 可读性 效率和低存储要求

## 顺序表的定义及主要特点
线性表的顺序存储成为顺序表,他用一组地址连续的存储单元依次存储线性表中的数据元素,从而使得在逻辑上相邻的两个元素在物理位置上也相邻
主要特点: 
1. 随机存储
2. 存储密度高,每个节点只存储数据元素
3. 顺序表逻辑上相邻的元素在物理地址上也相邻,所以插入和删除需要删除大量元素

## 什么是头结点
在单链表的第一个结点之前附加的一个结点称之为头结点

## 头结点和头指针的区分
无论带不带头结点,头指针始终指向链表的第一个结点,而头结点是带头结点的单链表的第一个结点,结点内通常不带存储信息

## 引入头结点的好处
由于开始结点的位置被存放在头结点的指针域中,所以在链表的第一个位置的操作和其他位置上的操作一致,无需进行特殊处理
无论链表是否为空,其头指针都指向头结点的非空指针,因此空表和非空表的操作也得到了统一

## 单链表的优缺点
1. 存取方式: 链表只能从表头开始顺序存取元素
2. 逻辑结构和物理结构: 在逻辑结构上相邻的元素,其物理存储位置不一定相邻
3. 查找删除操作: 链表的插入删除操作,只需要改变相关结点的指针域即可,由于每一个结点都带有指针域,因此在存储空间上要比顺序存储付出的代价大,存储密度不高
4. 顺序存储在静态和动态存储分配下,都十分麻烦,容易内存溢出或浪费大量空闲存储空间,链式存储的结点空间只有在需要时才会分配,操作灵活,高效

## 头插法和尾插法有什么区别
头插法建立单链表时,读入的数据和生成链表中的元素的顺序是相反的,尾插法得到的链表中元素的顺序和输入顺序相同

## 循环队列公式
队满条件: (q.rear+1)%maxsize == q.front;
队空条件: q.rear == q.front
队列中元素的数量: (q.rear-front+maxsize)%maxsize

## 什么叫做递归
在一个函数,过程或数据结构的定义中又应用了自身,则这个函数,过程或数据结构称为是递归定义的

## 数组和线性表的关系
数组是由n个相同类型的数据元素构成的有限序列,每个元素成为一个数组元素
数组是线性表的推广,一维数组可视为一个线性表,二维数组可视为元素是线性表的线性表

## 二叉树和度为2的有序树的区别
1. 度为2的有序树,至少有三个节点,二叉树可以为空
2. 度为2的有序树的孩子结点的左右次序是相对于另一个孩子结点而言的,若某个结点的孩子只有一个孩子结点,那么这个孩子结点就无需分为左右,而二叉树中无论孩子结点是否为二,其左右次序都是固定的

## 满二叉树
一棵高度为h,且含有2<sup>h</sup>-1个结点的二叉树称为满二叉树

## 完全二叉树
高度为h,有n个结点的二叉树,当且仅当其每个结点都与高度为h的满二叉树中编号为1~n的结点一一对应时,称为完全二叉树

## 二叉排序树
一棵二叉树,如果是空二叉树,或者是具有如下性质的二叉树,左子树上所有结点的关键字都小于根节点的关键字,右子树的关键字都大于根节点的关键字,并且左右子树又各是一棵二叉排序树

## 平衡二叉树
树上任意节点的左子树和右子树的深度之差不超过一

## 二叉树的遍历
二叉树的遍历是指按某条搜索路径访问树中的每个结点,使得每个结点均被访问一次

## 树的存储结构-双亲表示法
采用连续空间存储每个节点,同时在每个结点中增设一个伪指针,指示其双亲结点在数组中的位置

## 孩子表示法
将每个结点的孩子结点都用单链表链接起来,形成一个线性结构,此时n个结点就有n个孩子链表

## 孩子兄弟表示法
孩子兄弟表示法又称为二叉树表示法,即以二叉链表为存储结构,孩子兄弟表示法使得每个结点包括三个部分的内容:结点值,指向结点第一个孩子结点的指针,以及指向结点下一个兄弟节点的指针
最大的优点是方便实现树转换为二叉树的操作,易于查找结点的孩子节点,缺点是从当前节点查找其双亲结点比较麻烦

## 哈夫曼树及哈夫曼编码
在含有n个带权叶子结点的二叉树中,其中带权路径长度最小的二叉树称为哈夫曼树,即最优二叉树
哈夫曼编码是一种广泛应用并且非常有效的数据压缩编码

## 空表空树
在数据结构中,线性表可以没有数据元素,称为空表
树中没有结点,称为空树
图中不能没有结点

## 完全图
在无向图中,任意两个顶点之间都存在边,称该图为完全图,含有n个顶点的完全图有n(n-1)/2条边
在有向图中,任意两个顶点之间都存在方向相反的两条弧,则称为有向完全图,含有n个顶点的有向完全图有n(n-1)条有向边

## 生成树 生成森林
连通图的生成树是包含图中全部顶点的一个极小连通子图
在非连通图中,连通分量的生成树构成了非连通图的生成森林

## 结点的度 入度出度
图中每个顶点的度定义为以该顶点为一个端点的边的边的数目
入度: 在有向图中,顶点v的入度为以v为终点的有向边的数目
出度: 在有向图中,顶点v的出度为以v为起点的有向边的数目

## 图的遍历
图的遍历是指从图中的某一个顶点出发,按照某种搜索方法沿着图中的边对图中所有的顶点访问一次

## 最短路径
图为带权图时,把一个顶点v0到图中其余任意一个顶点vi的一条路径所经过的权值之和,定义为该路径的带权路径长度,将带权路径长度最短的那条路径称为最短路径

## 有向无环图
若一个有向图中不存在环,则称为有向无环图DAG

## AOV
若用DAG表示一个工程,其顶点表示活动,有向边(vi,vj)表示活动vi必须先于vj进行的关系,则称这种有向图是顶点表示活动的网络,称为AOV网

## AOE网
在带权有向图中,顶点表示事件,有向边表示活动,边上的权值表示完成该活动需要的开销,则称这种有向图为用边表示活动的网络,简称为AOE网

## 关键路径
在AOE网中,从源点到汇点的所有路径中,具有最大路径长度的路径称为关键路径,关键路径上的活动称为关键活动

## 顺序查找的优缺点
缺点: 顺序查找的缺点是当n较大时,平均查找长度较大,效率低
优点: 对数据的存储没有要求,顺序存储或链式存储都可以

## 什么是折半查找
又称为二分查找,将给定的key值与表中间位置的关键字比较,若相等则查找成功,若不相等,则所需查找的元素只能在中间元素的前半侧或后半侧

## 什么是分块查找
又称为索引顺序查找,将查找表划分为多个块,块内元素可以无序,但块之间必须有序,即第i个块中的最大关键字必须小于第i+1个块中的最大关键字,因此建立一个索引表,索引表中的每个元素含有各块中最大关键字和各块中第一个元素的地址,查找过程分为两步,首先在索引表中确定待查元素所在的块,第二步在块内进行查找

## 什么叫B树
b树为所有结点的平衡因子都等于0的多路平衡查找树
b树中所有结点的孩子结点数的最大值称为B树的阶,通常用m来表示
1. 树中每个结点至多有一个m棵子树(即至多有m-1个关键字)
2. 除根节点外的所有非叶子结点,都至少有m/2(向上取整)棵子树
3. 所有的叶子结点都在同一层次上,并且不带信息,可以看作是查找失败结点

## 什么叫B+树
1. 每个分支结点最多有m个子树
2. 非叶根结点至少有两个子树,其他每个分支结点至少有m/2(向上取整)棵子树
3. 结点的子树数量与结点关键字相等
4. 所有叶结点包含所有关键字及指向相应记录的指针
5. 所有分支节点中仅包含他的各个子节点中关键字的最大值,及指向子节点的指针

## B+树和B树的差异
1. 在B+树中具有n个关键字的结点含有n棵子树,即每个关键字对应一棵子树,而在B树中,具有n个关键字的结点有n+1棵子树
2. 在B+树中,每个结点(非根内部结点)的关键字数n范围为 m/2(向上取整)<= n <=m
3. 在B树中,每个结点(非根内部结点)的关键字数n范围为 m/2(向上取整)-1 <= n <= m-1
4. 在B+树中,叶节点包含信息,非叶节点起索引作用,而在B树中,叶节点包含的关键字和其他结点的关键字是不重复的

## 什么散列表的填充因子
表中记录数n/散列表长度m 

## 什么是算法的稳定性
若待排序表中有两个元素Ri和Rj,其对应的关键字keyi == keyj,若使用某一排序算法后,两个元素的相对位置没有发生改变,则称为这个算法是稳定的

## 什么是内部排序,什么是外部排序
内部排序: 排序期间元素全部存放在内存中排序
外部排序: 排序期间元素无法全部同时存放在内存中,必须在排序过程中,根据要求不断在内外存之间移动的排序

## 简述快速排序的思想
快速排序基于分治思想,在待排序列[1...n]中任取一个元素pivot作为基准,通过一趟排序将待排序表划分为两个单独部分[1...k-1]和[k+1...n],pivot放在了最终的位置,这个过程称为一趟快速排序,之后递归的对两个子表重复上述过程,直到所有元素都有序

## 什么是基数排序
基数排序是一种特殊的排序方法,它不基于比较进行排序,而采用多关键字排序思想(基于关键字的各位的大小进行排序)借助"分配"和"收集"两种操作对单逻辑关键字进行排序

## 各种排序算法的平均时间复杂度,稳定性
|     算法     |       最好情况       |       平均情况       |       最坏情况       |      空间复杂度      | 稳定  |
| :----------: | :------------------: | :------------------: | :------------------: | :------------------: | :---: |
| 直接插入排序 |         O(n)         |   O(n<sup>2</sup>)   |   O(n<sup>2</sup>)   |         O(1)         |  是   |
|   冒泡排序   |         O(n)         |   O(n<sup>2</sup>)   |   O(n<sup>2</sup>)   |         O(1)         |  是   |
| 简单选择排序 |   O(n<sup>2</sup>)   |   O(n<sup>2</sup>)   |   O(n<sup>2</sup>)   |         O(1)         |  否   |
|   希尔排序   |                      |                      |                      |         O(1)         |  否   |
|   快速排序   | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) |   O(n<sup>2</sup>)   | O(nlog<sub>2</sub>n) |  否   |
|    堆排序    | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) |         O(1)         |  否   |
| 2路归并排序  | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) |         O(n)         |  是   |
|   基数排序   |      O(d(n+r))       |      O(d(n+r))       |      O(d(n+r))       |         O(r)         |  是   |

## 排序趟数和初始状态无关的排序方法是
直接插入 简单选择排序 基数排序

## 内部排序中,哪几种排序每一趟排序结束都至少能确定一个元素的最终位置
简单选择 快速排序 堆排序
