# Map/Set/优先队列 的补充



最近发现 Set-Map-优先队列 有许多好用的使用方法,整理一下补充的知识点8!
<!--more-->

# Set
## 文档介绍
1. 在set中，元素的value也标识它(value就是key，类型为T)，并且每个value必须是唯一的
2. set容器通过key访问单个元素的速度通常比unordered_set容器慢，但它们允许根据顺序对子集进行直接迭代。
3. 与map/multimap不同，map/multimap中存储的是真正的键值对，set中只放value，但在底层实际存放的是由构成的键值对。
4. 使用set的迭代器遍历set中的元素，可以得到有序序列,set中的元素**默认按照小于来比较**(less<type>())
5.  set中查找某个元素，时间复杂度为：log<sub>2</sub>n
6.  set中的元素不允许修改(底层使用二叉搜索树实现的）
7.  set中的底层使用二叉搜索树(红黑树)来实现。

**因此利用set自带的排序功能,就可以充当堆(取最大值和最小值)**

set与multiset最大的区别是multiset中的元素可以重复
**使用 multiset 查找重复元素时，找到的是搜索二叉树中序访问的第一个元素**

## multiset 自定义排序
利用自定义类型
```c++
struct rec{
    int x,y;
};
multiset<rec>h;
```
但是multiset并不知道如何去比较一个自定义的类型,此时可以定义multiset里面rec类型变量之间的小于关系的含义（这里以x为第一关键字为例）

**定义一个比较类cmp，cmp内部的operator函数的作用是比较rec类型a和b的大小(以x为第一关键字，y为第二关键字)**
```c++
struct cmp{
    bool operator()(const rec&a,const rec&b){
        return a.x<b.x||a.x==b.x&&a.y<b.y;
    }
};
```
然后将语句
```c++
multiset<rec> h
// 改为
multiset<rec,cmp> h;
```
就告诉了序列 h 如何去比较里面的元素(重载运算符)

此时rec以及multiset的定义部分完整代码为:
```c++
struct rec{
    int x,y;
};
struct cmp{
    bool operator()(const rec&a,const rec&b){
        return a.x<b.x||a.x==b.x&&a.y<b.y;
    }
};
multiset<rec,cmp> h;
```

# Map
Map也是利用的红黑树-RB树(Red-Black Tree)。
map和set的树建立之时就会自动排好序
默认的比较函数 map(T1, T2, less<type>) 即**按key的升序(从小到大)排列**,当然也可以[通过重载函数设置为根据value排序](https://www.cnblogs.com/stay-alive/p/8215400.html)
查找效率为O(log<sub>2</sub>n)

对于map的 insert 我之前一直用的重载operator [] 虽然该方法在效率上偏慢一些,但可以替换元素键值
不过最好还是构造pair insert吧
```c++
map<int, string> m;
/* 方法 1*/
pair<int, string> shit = make_pair(1,"value1");
m.insert(shit);
/* 方法 2*/
m.insert(make_pair(1, "value1"));
/* 方法 3*/
m.insert(pair<int, string>(1,"value1"));
/* 方法 3*/
m.insert(map<int, string>::value_type (1,"value1"));
```

# priority_queue
另外说到set和map,不得不提一下优先队列,都要熟练用鸭! (文末彩蛋)
优先队列本质上是用堆实现的
**注意优先队列中的greater和less排序顺序,与正常是反过来的(因为队列头部出队)**

## 基本优先队列
```c++
#include<queue>
#include<cstdio>
using namespace std;
int date[5];

int main()
{
    /*
    定义：priority_queue<Type, Container, Functional>
    Type 就是数据类型，Container 就是容器类型（Container必须是用数组实现的容器，比如vector,deque等等，但不能用 list。STL里面默认用的是vector），Functional 就是比较的方式。
    */
    priority_queue<int> Q; //默认为最大值(大顶堆)优先
    priority_queue<int,vector<int>,greater<int> > Q1; //最小值优先
    priority_queue<int,vector<int>,less<int> > Q2; //最大值优先

    for(int i=1;i<=3;i++)
    {
        scanf("%d",&date[i]);
        Q.push(date[i]); //插入元素
    }
    while(!Q.empty())
    {
        int x = Q.top(); //读取队头元素
        Q.pop(); //弹出元素
        printf("%d\n",x);
    }
    return 0;
}
```

## 使用 pair 作为优先队列元素
排序规则：pair的比较，先比较第一个元素，第一个相等比较第二个。

## 使用 自定义类型 作为优先队列元素
```c++
#include <iostream>
#include <queue>
using namespace std;
/* 方法1 运算符重载 < */
struct tmp1
{
    int x;
    tmp1(int a) {x = a;}
    bool operator < (const tmp1& a) const
    {
        //return x < a.x; //大顶堆
    }
};
/* 方法2 重写仿函数 */
struct tmp2
{
    bool operator() (tmp1 a, tmp1 b)
    {
        return a.x < b.x; //大顶堆
    }
};

int main()
{
    tmp1 a(1);
    tmp1 b(2);
    tmp1 c(3);
    priority_queue<tmp1> d;
    d.push(b);
    d.push(c);
    d.push(a);
    while (!d.empty())
    {
        cout << d.top().x << '\n';
        d.pop();
    }
    cout << endl;

    priority_queue<tmp1, vector<tmp1>, tmp2> f;
    f.push(b);
    f.push(c);
    f.push(a);
    while (!f.empty())
    {
        cout << f.top().x << '\n';
        f.pop();
    }
}
/*
输出结果:
3
2
1
 
3
2
1
*/
```

