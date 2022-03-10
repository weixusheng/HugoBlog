# C++ Part-4


算法笔记知识点汇总-C++ STL<br>
7.18更新完成
<!--more-->

这里为了精简一点,所以只记录一些个人觉得重要的东西,由于是第一遍学,所以相对基础

在STL容器中,只有vector和string支持 *(it+i)的访问方式
# vector
向量,变长数组
对于定义STL容器,方法都一样
```c++
vector<int> vector1;
```
迭代器,方法也一样
```c++
vector<int> :: iterator it;
```
如果其中填装的结构也是容器,则需要注意,在括号后要加入一个空格,以区分 << 否则编译会出错
```c++
vector <vector<int> > vector1;
```
## 函数操作
push_back(): 在vector后面添加一个元素
pop_back(): 删除vector尾元素
size(): 获得元素个数
clear(): 清空元素
insert(it, x): 在迭代器it处,插入元素x
erase(it): 删除迭代器处的元素
erase(first, last): 删除first到last之间的元素(前闭后开,first,last为迭代器)

# set
集合:内部自动有序,且不含重复元素的集合
底层实现为红黑树,也可以使用unordered_set容器,底层实现为散列,速度比set快很多
## 函数操作
insert(x): 将x元素插入到set中
find(value): 查找值为value的元素,并返回迭代器
erase(x): 删除迭代器对应的元素
erase(value): 删除值为value的元素
erase(first,last)
size()
clear()

# string
c++对char[]进行封装,从而产生string类型
对于在printf中输出string,可以使用c_str()函数
```c++
string s = "hello world";
printf("%s", s.c_str());
```
对于string的加法,代表两个string进行拼接
而对于两个string的比较符号,比较规则为字典序,与char[]的比较规则相同

## 操作函数
length(),size()
insert(pos,string)
insert(it1,it2,it3): it为被插入字符串的欲插入位置,it2,it3为待插入字符串的起始和终止位置迭代器(前闭后开)
erase(it),erase(first,last)
erase(pos, length): 从起始位置pos开始,删除length个元素
substr(pos, length): 从pos位置开始,截取length长度元素,并返回(字符串并不会发生改变)
string::npos 是一个常数,作为find函数失配时的返回值
find(str2): 如果str2为str的字串,返回其在str中的第一次出现的位置
```c++
if(str.find(str2) != string::npos){
    /*code*/
}
```  
replace(pos, length, str2): 将str从pos位置开始,长度为length的字串替换为str2

# map
映射,字典,键值唯一
```c++
map<string int> mp;
map<set<int> string> mp2;
```

访问键和对应的值
```c++
it->first //访问键
it->second //访问值
```
由于map底层为红黑树实现,因此键会从大到小进行排列自动排序

## 操作函数
find(key)
erase(it), erase(key), erase(first,last)
size()
clear()

# queue
队列是一种限制性的数据结构,因此只能通过front()访问队首元素,back()访问队尾元素
在使用front()和back()函数前,必须用empty()判断队列是否为空
## 操作函数
push(x) 队尾入队
pop() 队首出队
front() 获得队首元素
back() 获得队尾元素
empty() 判断queue是否为空
size() 返回队列长度

# priority_queue
优先队列,底层用堆来实现,在任何时候push进队列,堆(heap)都会调整结构,保证队首元素为优先级最大
头文件: queue
和队列不同,priority_queue没有front()和back()函数,只能通过top()来访问队首元素(堆顶元素)
## 操作函数
push()
top()
pop()
empty()
size()

## 优先级的设置
```c++
priority_queue<int, vector<int>, less<int> > q;
priority_queue<int, vector<int>, greator<int> > p;
``` 
第一个参数vector-int填写的是底层数据结构(heap)的容器,如果第一个参数为char,则第二个参数为vector-char
第三个参数为是对第一个参数的比较类,less-int表示数字越大的优先级越大

### 结构体优先级
```c++
struct fruit{
    string name;
    int price;
    friend bool operator < (fruit f1, fruit f2){
        return f1.price < f2.price; //价格高的水果,优先级高
    }
};
priority_queue<fruit> q; //结构体内部会自动排序,而不需要额外写别的比较函数
```
friend为友元,结构体内对 "<" 进行了重载(重载">"会报错)
```c++
f1 == f2 等价于判断 !(f1<f2) && !(f2<f1)
```
其中对于小于号的重载,结果与sort函数中的cmp排列是相反的
```c++
/*cmp*/
return a>b; //递减排序,大的元素在前面
/* < 重载*/
return a>b; //递增排序,小的元素在前面
```
将比较函数写在结构体外面
```c++
struct fruit{
    string name;
    int price;
}f1,f2,f3;
struct cmp{
    bool operator()(fruit f1, fruit f2){
        return f1.price > f2.price;
    }
}
priority_queue<fruit, vector<fruit>, cmp> q;
```
如果结构体内数据较为庞大,应该使用引用来提高效率,此时比较类的参数应该加上"const"和"&"
```c++
friend bool operator < (const fruit &f1, const fruit &f2){
    return f1.price > f2.price;
}
bool operator() (const fruit &f1, const fruit &f2){
    return f1.price > f2.price;
}
```

# stack
## 操作函数
push()
top()
pop()
empty()
siza()

# pair
头文件 map
内部有两个元素的结构体,分别对应first和second
```c++
pair<string, int> p;
pair<string, int> p("haha", 5); //初始化数据
pair<string, int>("haha", 5) //创建临时变量
make_pair("haha", 5); //创建临时变量
```
pair经常用作插入map中
```c++
map<string, int> mp;
mp.insert(make_pair("haha",5));
for(auto it = mp.begin(); it!=mp.end(); it++){
    cout << it->first << it->second;
}
```
## 比较函数
pair可以使用 ==,!=,<,<=,>,>=进行比较,比较规则是首先对first的大小作为标准,只有当first相等时才会去判断second的大小

# algorithm头文件下的常用函数
## max()
返回最大值 max(x,y)
三个值取最大,利用嵌套 max(x,max(y,z))
## min()
返回最小值
## abs()
返回绝对值,需要注意的是,abs只能用于整型int,浮点数的绝对值需要使用fabs()函数

## swap()
swap(x,y)交换x,y两个变量的值

## reverse()
将数组指针在[it1, it2)之间的元素或容器的迭代器在[it1, it2)之间的元素进行反转
注意区间,前闭后开
```c++
int a[10] = {10,11,12,13,14,15};
reverse(a, a+4); //将 a[0]到a[3] 的元素进行反转
string a = "abcdefg";
reverse(a.begin()+2, a.begin()+5); //对 a[2]到a[4] 的元素进行反转
```

## fil()
可以将数组或容器的某一区间赋值为某个相同得值
```c++
int a[5] = {1,2,3,4,5};
fill(a, a+5, 233); //将 a[0]到a[4] 均赋值为233
```

## lower_bound()和upper_bound()
首先，lower_bound和upper_bound是C++ STL中提供的非常实用的函数。其操作对象可以是vector、set以及map
lower_bound返回值一般是>= 给定val的最小指针（iterator）
upper_bound返回值则是 > 给定val的最小指针（iterator）
```c++
#include <iostream>     // std::cout
#include <algorithm>    // std::lower_bound, std::upper_bound, std::sort
#include <vector>       // std::vector

using namepace std;

int main () {
  int myints[] = {10,20,30,30,20,10,10,20};
  vector<int> v(myints,myints+8);           // 10 20 30 30 20 10 10 20
  sort (v.begin(), v.end());                // 10 10 10 20 20 20 30 30
  vector<int>::iterator low,up;
  low = lower_bound (v.begin(), v.end(), 20); 
  up = upper_bound (v.begin(), v.end(), 20); 
  cout << "lower_bound at position " << (low- v.begin()) << '\n';
  cout << "upper_bound at position " << (up - v.begin()) << '\n';
  return 0;
}
```
