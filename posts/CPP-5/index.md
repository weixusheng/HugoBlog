# C++ transform()


C++ transform()函数用法
<!--more-->

transform函数的作用是：**将某操作应用于指定范围的每个元素**
transform函数有两个重载版本：
# 重载版本-1 
```c++
transform(first,last,result,op);
//first是容器的首迭代器，last为容器的末迭代器，result为存放结果的容器，op为要进行操作的一元函数对象或sturct、class。
```
利用transform函数将一个给定的字符串中的小写字母改写成大写字母，并将结果保存在一个叫second的数组里，原字符串内容不变。
```c++
#include <iostream>
#include <algorithm>
using namespace std;

char op(char ch)
{
    if(ch>='A'&&ch<='Z')
        return ch+32;
    else
        return ch;
}
int main()
{
    string first,second;
    cin>>first;
    second.resize(first.size());
    transform(first.begin(),first.end(),second.begin(),op);
    cout<<second<<endl;
    return 0;
}
```
## 利用::func()实现转换
```c++
transform(str.begin(),str.end(),str.begin(),::tolower);
transform(str.begin(),str.end(),str.begin(),::toupper);
```

# 重载版本-2
```c++
transform(first1,last1,first2,result,binary_op);
//first1是第一个容器的首迭代 器，last1为第一个容器的末迭代器，first2为第二个容器的首迭代器，result为存放结果的容器，binary_op为要进行操作的二元函数 对象或sturct、class。
```
注意：第二个重载版本必须要保证两个容器的元素个数相等才行，否则会抛出异常。
给你两个vector向量（元素个数相等），请你利用transform函数将两个vector的每个元素相乘，并输出相乘的结果。
```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

void print(int &elem){cout<<elem<<" ";}
int op(int a,int b){return a*b;}

int main()
{
    vector <int> A,B,SUM;
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
    {
        int t;
        cin>>t;
        A.push_back(t);
    }
    for(int i=0;i<n;i++)
    {
        int t;
        cin>>t;
        B.push_back(t);
    }
    SUM.resize(n);
    transform(A.begin(),A.end(),B.begin(),SUM.begin(),op);
    for_each(SUM.begin(),SUM.end(),print);
    return 0;
}
```
