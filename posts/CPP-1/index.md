# C++ Part-1


从一开始刷OJ,leetcode到PAT,一直用的C
晚上去听了两小时c++刷PAT题的教程,发现原来...c++这么爽!
我不行了,我是菜鸡,我用不起c,以我现在的实力,就一个char数组或者哈希我能玩玩,别的工具也太少了
第一部分: 基础概念
<!--more-->

今天看了一半的简单教程,这一个个知识点也太熟悉了,跟c#,java差不多嘛(都是面向对象的,不然呢)找到了曾经初恋的感觉
废话不多说,今天把要点记下来,只记个人觉得重要的知识点

# 枚举类型
创建枚举，需要使用关键字 enum。枚举类型的一般形式为：
```c++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
```
如果枚举**没有初始化**, 即省掉"=整型常数"时, 则**从第一个标识符开始**。
例如，下面的代码定义了一个颜色枚举，变量c的类型为color。最后，c被赋值为 "blue"。
```c++
enum color { red, green, blue } c;
c = blue;
```
默认情况下，第一个名称的值为0，第二个名称的值为1，第三个名称的值为2，以此类推。但是，您也可以给名称赋予一个特殊的值，只需要添加一个初始值即可。例如，在下面的枚举中，green的值为5。
```
enum color { red, green=5, blue };
```
在这里，blue的值为6，因为默认情况下，每个名称都会比它前面一个名称大1，但**red的值依然为0**。

# 有符号整数和无符号整数修饰符
```c++
#include <iostream>
using namespace std;

int main()
{
   short int i;           // 有符号短整数
   short unsigned int j;  // 无符号短整数
   j = 50000;
   i = j;
   cout << i << " " << j;
   return 0;
}
```
当上面的程序运行时，会输出下列结果：
```c++
-15536 50000 //无符号短整数 50,000 的位模式被解释为有符号短整数 -15,536
```

# static 存储类
static 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。
使用static修饰局部变量可以在函数调用之间保持局部变量的值。
当static修饰全局变量时，会使变量的作用域限制在声明它的文件内。

# extern 存储类
这个之前在c中有,现在再记录一遍
extern 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的。当您使用 'extern' 时，对于无法初始化的变量，会把变量名指向一个**之前定义过的存储位置**。
当您有多个文件且定义了一个可以在其他文件中使用的全局变量或函数时，可以在其他文件中使用 extern 来得到已定义的变量或函数的引用。
可以这么理解，extern 是用来在另一个文件中声明一个全局变量或函数。

# 函数参数
## &引用
引用可以理解为: 为当前数据起了一个**别名**,在操纵这个别名时,就是在操作这个数据本身
另外引用的&符号和取地址的&区别很大,可以看作是两种功能的符号(这个需要特别注意)
```c++
#include <iostream>
 
using namespace std;
 
int main ()
{
   // 声明简单的变量
   int    i;
   double d;
   // 声明引用变量
   int&    r = i;
   double& s = d;
   i = 5;
   cout << "Value of i : " << i << endl;
   cout << "Value of i reference : " << r  << endl;
   d = 11.7;
   cout << "Value of d : " << d << endl;
   cout << "Value of d reference : " << s  << endl;
   return 0;
}
/*
输出:
Value of i : 5
Value of i reference : 5
Value of d : 11.7
Value of d reference : 11.7
*/
```

## 引用调用
向函数传递参数的引用调用方法，把引用的地址复制给形式参数。在函数内，该引用用于访问调用中要用到的实际参数。
**修改形式参数会影响实际参数**
按引用传递值，参数引用被传递给函数，就像传递其他值给函数一样。因此相应地，在下面的函数 swap() 中，您需要声明函数参数为引用类型，该函数用于交换参数所指向的两个整数变量的值。
```c++
#include <iostream>
using namespace std;

void swap(int &x, int &y);
int main ()
{
   // 局部变量声明
   int a = 100;
   int b = 200;
   cout << "交换前，a 的值：" << a << endl;
   cout << "交换前，b 的值：" << b << endl;
   /* 调用函数来交换值 */
   swap(a, b);
   cout << "交换后，a 的值：" << a << endl;
   cout << "交换后，b 的值：" << b << endl;
   return 0;
}
void swap(int &x, int &y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
  
   return;
}
```

## C++ 引用作为返回值
当函数返回一个引用时,则返回一个指向返回值的隐式指针,函数就可以放在赋值语句的左边
```c++
#include <iostream>
using namespace std;
 
double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
double& setValues( int i )
{
  return vals[i];   // 返回第 i 个元素的引用
}
// 要调用上面定义函数的主函数
int main ()
{
   setValues(1) = 20.23; // 改变第 2 个元素
   setValues(3) = 70.8;  // 改变第 4 个元素
   return 0;
}
```

## 函数参数默认值
只需要在参数中进行初始化值就完事了
当调用函数时，如果实际参数的值留空，则使用这个默认值
```c++
int sum(int a, int b=20)
{
  /*code*/
}
```

# C++ 随机数
生成随机数需要调用两个函数 srand()和rand()
下民使用time()函数来获取系统时间的秒数，并通过调用 rand() 函数来生成随机数：
```c++
#include <iostream>
#include <ctime>
#include <cstdlib>
 
using namespace std;
int main ()
{
   int i,j;
   // 设置种子
   srand( (unsigned)time( NULL ) );
   /* 生成 10 个随机数 */
   for( i = 0; i < 10; i++ )
   {
      // 生成实际的随机数
      j= rand();
      cout <<"随机数： " << j << endl;
   }
   return 0;
}
```

# C++ 字符串
## C-字符串
以下函数适用于char数组构成的字符串
1. strcpy(s1, s2);
复制字符串 s2 到字符串 s1。
2. strcat(s1, s2);
连接字符串 s2 到字符串 s1 的末尾。
3. strlen(s1);
返回字符串 s1 的长度。
4. strcmp(s1, s2);
如果 s1 和 s2 是相同的，则返回 0；如果 s1 < s2 则返回值小于 0；如果 s1 > s2 则返回值大于 0。
5. strchr(s1, ch);
返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。
6. strstr(s1, s2);
返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置。

## C++ 中的 String 类
在以后标准库中进行讲解

# 变量值的交换函数的优化
这里利用了数异或的性质，一个数与其本身异或等于 0，0 与一个数异或不改变该数。说到 swap
```c++
int swap(int& a, int& b)
{
    int temp;
    temp = a ^ b;
    a = temp ^ a;
    b = temp ^ b;
    return 0;
}
```
