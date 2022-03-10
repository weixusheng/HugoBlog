# C语言程序设计 Part-1


今天补习了一下c语言(重新再学习一遍c),毕竟基础要牢靠
也为了下一步刷PAT做基础,这里记录新得
今天心态又崩了...感觉一直以来自己就是一个快乐与崩溃的结合体
无尽的失落和失望,像是一个深渊,不过明天醒来又是新的一天
<!--more-->

# void 类型
 - 函数返回为空
C 中有各种函数都不返回值，或者您可以说它们返回空。不返回值的函数的返回类型为空。例如 void exit (int status);
 - 函数参数为空
C 中有各种函数不接受任何参数。不带参数的函数可以接受一个 void。例如 int rand(void);
 - 指针指向 void
类型为 void * 的指针代表对象的地址，而不是类型。例如，内存分配函数 void *malloc( size_t size ); 返回指向 void 的指针，可以转换为任何数据类型。

# 小数类型
## float类型
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-4/1.png)

## double类型
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-4/2.png)

# C 中的变量声明-extern声明
变量声明向编译器保证变量以指定的类型和名称存在，这样编译器在不需要知道变量完整细节的情况下也能继续进一步的编译。变量声明只在编译时有它的意义，在程序连接时编译器需要实际的变量声明。
变量的声明有两种情况：
1. 一种是需要建立存储空间的。例如：int a 在声明的时候就已经建立了存储空间。
2. 另一种是不需要建立存储空间的，通过使用extern关键字声明变量名而不定义它。 
3. 例如：extern int a 其中变量 a 可以在别的文件中定义的。
除非有extern关键字，否则都是变量的定义。
```c
extern int i; //声明，不是定义
int i; //声明，也是定义
```
实例
尝试下面的实例，其中，变量在头部就已经被声明，但是定义与初始化在主函数内：
```c
#include <stdio.h>
// 函数外定义变量 x 和 y
int x;
int y;
int addtwonum()
{
    // 函数内声明变量 x 和 y 为外部变量
    extern int x;
    extern int y;
    // 给外部变量（全局变量）x 和 y 赋值
    x = 1;
    y = 2;
    return x+y;
}
 
int main()
{
    int result;
    // 调用函数 addtwonum
    result = addtwonum();
    
    printf("result 为: %d",result);
    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
result 为: 3
```
如果需要在一个源文件中引用另外一个源文件中定义的变量，我们只需在引用的文件中将变量加上 extern 关键字的声明即可。
addtwonum.c 文件代码：
```c
#include <stdio.h>
/*外部变量声明*/
extern int x ;
extern int y ;
int addtwonum()
{
    return x+y;
}
```
test.c 文件代码：
```c
#include <stdio.h>
/*定义两个全局变量*/
int x=1;
int y=2;
int addtwonum();
int main(void)
{
    int result;
    result = addtwonum();
    printf("result 为: %d\n",result);
    return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
$ gcc addtwonum.c test.c -o main
$ ./main
result 为: 3
```

# C 常量
## 整数常量
整数常量可以是十进制、八进制或十六进制的常量。
前缀指定基数：
 - 0x 或 0X 表示十六进制
 - 0表示八进制
 - 不带前缀则默认表示十进制。

整数常量也可以带一个后缀，后缀是 U 和 L 的组合，U 表示无符号整数（unsigned），L 表示长整数（long）。后缀可以是大写，也可以是小写，U 和 L 的顺序任意。
```c
85         /* 十进制 */
0213       /* 八进制 */
0x4b       /* 十六进制 */
30         /* 整数 */
30u        /* 无符号整数 */
30l        /* 长整数 */
30ul       /* 无符号长整数 */
```

## 浮点常量
浮点常量由整数部分、小数点、小数部分和指数部分组成。您可以使用小数形式或者指数形式来表示浮点常量。
当使用小数形式表示时，必须包含整数部分、小数部分，或同时包含两者。当使用指数形式表示时， 必须包含小数点、指数，或同时包含两者。带符号的指数是用 e 或 E 引入的。
```c
3.14159       /* 合法的 */
314159E-5L    /* 合法的 */
```

# C 存储类
存储类定义 C 程序中变量/函数的范围（可见性）和生命周期。这些说明符放置在它们所修饰的类型之前。下面列出 C 程序中可用的存储类：
 - auto
 - register
 - static
 - extern

## auto 存储类
auto 存储类是所有局部变量**默认**的存储类。
```c
{
   int mount;
   auto int month;
}
```
上面的实例定义了两个带有**相同存储类**的变量，auto 只能用在函数内，即 auto 只能修饰局部变量。

## static 存储类
static 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用 static 修饰局部变量可以在函数调用之间保持局部变量的值。
static 修饰符也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。
全局声明的一个 static 变量或方法可以被任何函数或方法调用，只要这些方法出现在跟 static 变量或方法同一个文件中。
以下实例演示了 static 修饰全局变量和局部变量的应用：
```c
#include <stdio.h>
/* 函数声明 */
void func1(void);
static int count=10;        /* 全局变量 - static 是默认的 */
int main()
{
  while (count--) {
      func1();
  }
  return 0;
}
void func1(void)
{
/* 'thingy' 是 'func1' 的局部变量 - 只初始化一次
 * 每次调用函数 'func1' 'thingy' 值不会被重置。
 */                
  static int thingy=5;
  thingy++;
  printf(" thingy 为 %d ， count 为 %d\n", thingy, count);
}
```
实例中 count 作为全局变量可以在函数内使用，thingy 使用 static 修饰后，**不会在每次调用时重置**。产生结果
```
 thingy 为 6 ， count 为 9
 thingy 为 7 ， count 为 8
 thingy 为 8 ， count 为 7
 thingy 为 9 ， count 为 6
 thingy 为 10 ， count 为 5
 thingy 为 11 ， count 为 4
 thingy 为 12 ， count 为 3
 thingy 为 13 ， count 为 2
 thingy 为 14 ， count 为 1
 thingy 为 15 ， count 为 0
```

## extern 存储类
extern 存储类用于提供**一个全局变量的引用**，全局变量对**所有的程序文件**都是可见的。当您使用 extern 时，对于无法初始化的变量，会把**变量名指向一个之前定义过的存储位置**。
当您有多个文件且定义了一个可以在其他文件中使用的全局变量或函数时，可以在其他文件中使用 extern 来得到已定义的变量或函数的引用。可以这么理解，**extern 是用来在另一个文件中声明一个全局变量或函数**。
extern 修饰符通常用于当有两个或多个文件共享相同的全局变量或函数的时候，如下所示：
第一个文件：main.c
实例
```c
#include <stdio.h>
int count ;
extern void write_extern();
int main()
{
   count = 5;
   write_extern();
}
```
第二个文件：support.c
```c
#include <stdio.h>
extern int count;
void write_extern(void)
{
   printf("count is %d\n", count);
}
```
在这里，第二个文件中的 extern 关键字用于声明已经在第一个文件 main.c 中定义的 count。现在 ，编译这两个文件，如下所示：
```c
 $ gcc main.c support.c
```
这会产生 a.out 可执行程序，当程序被执行时，它会产生下列结果：
```c
count is 5
```

# 位运算符
位运算符作用于位，并逐位执行操作。&、 | 和 ^ 的真值表如下所示：
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/C-4/3.png)
假设如果 A = 60，且 B = 13，现在以二进制格式表示，它们如下所示：
```
A = 0011 1100
B = 0000 1101
-----------------
A&B = 0000 1100
A|B = 0011 1101
A^B = 0011 0001
~A  = 1100 0011
```
## 举例说明
假设变量 A 的值为 60，变量 B 的值为 13
### 运算符-按位与&
运算规则
```
0&0=0;   
0&1=0;    
1&0=0;     
1&1=1;
```
(A & B) 将得到 12，即为 0000 1100

### 运算符-按位或|
```
0|0=0;   
0|1=1;   
1|0=1;    
1|1=1;
```
(A | B) 将得到 61，即为 0011 1101

### 运算符-异或^
```
0^0=0;   
0^1=1;   
1^0=1;  
1^1=0;
```
(A ^ B) 将得到 49，即为 0011 0001

### 运算符-取反~
```
~1=0;   
~0=1;
```
(~A ) 将得到 -61，即为 1100 0011，一个有符号二进制数的补码形式。

### 运算符-二进制左移<<
A << 2 将得到 240，即为 1111 0000

### 运算符-二进制右移>>
A >> 2 将得到 15，即为 0000 1111

## 实例
```c

#include <stdio.h>
 
int main()
{
 
   unsigned int a = 60;    /* 60 = 0011 1100 */  
   unsigned int b = 13;    /* 13 = 0000 1101 */
   int c = 0;           
 
   c = a & b;       /* 12 = 0000 1100 */ 
   printf("Line 1 - c 的值是 %d\n", c );
 
   c = a | b;       /* 61 = 0011 1101 */
   printf("Line 2 - c 的值是 %d\n", c );
 
   c = a ^ b;       /* 49 = 0011 0001 */
   printf("Line 3 - c 的值是 %d\n", c );
 
   c = ~a;          /*-61 = 1100 0011 */
   printf("Line 4 - c 的值是 %d\n", c );
 
   c = a << 2;     /* 240 = 1111 0000 */
   printf("Line 5 - c 的值是 %d\n", c );
 
   c = a >> 2;     /* 15 = 0000 1111 */
   printf("Line 6 - c 的值是 %d\n", c );
}
```
输出结果
```c
Line 1 - c 的值是 12
Line 2 - c 的值是 61
Line 3 - c 的值是 49
Line 4 - c 的值是 -61
Line 5 - c 的值是 240
Line 6 - c 的值是 15
```

# switch case
如果在case语句中没有break;
会一直执行到下条语句,直到遇到break;
```
switch(int value){
    case 1:
        表达式1;
    case 2:
        表达式2;
        break;
    case 3:
        表达式3;
        break;
}
```
此时如果value=1,则会一直执行到表达式2,遇到break跳出

# 变量作用域
1. 全局变量与局部变量
在程序中，局部变量和全局变量的名称可以相同，但是在函数内，如果两个名字相同，**会使用局部变量值，全局变量不会被使用**
2. 形式参数
函数的参数，形式参数，被当作该函数内的局部变量，如果与全局变量同名它们会优先使用局部变量。下面是一个实例：

全局变量与局部变量在内存中的区别：
 - 全局变量保存在内存的全局存储区中，占用静态的存储单元；
 - 局部变量保存在栈中，只有在所在函数被调用时才动态地为变量分配存储单元。

# C enum(枚举)
枚举是 C 语言中的一种基本数据类型，它可以让数据更简洁，更易读。
枚举语法定义格式为：
```c
enum　枚举名　{枚举元素1,枚举元素2,……};
```
比如：一星期有 7 天，如果不用枚举，我们需要使用 #define 来为每个整数定义一个别名：
```c
#define MON  1
#define TUE  2
#define WED  3
#define THU  4
#define FRI  5
#define SAT  6
#define SUN  7
```
这个看起来代码量就比较多，接下来我们看看使用枚举的方式：
```c
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
```
注意：**第一个枚举成员的默认值为整型的 0，后续枚举成员的值在前一个成员上加 1**。我们在这个实例中把第一个枚举成员的值定义为 1，第二个就为 2，以此类推。
可以在定义枚举类型时改变枚举元素的值：
```c
enum season {spring, summer=3, autumn, winter};
```
没有指定值的枚举元素，其值为前一元素加 1。也就说 **spring 的值为 0，summer 的值为 3，autumn 的值为 4，winter 的值为 5**

## 枚举变量的定义
我们可以通过以下三种方式来定义枚举变量

### 先定义枚举类型，再定义枚举变量
```c
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
enum DAY day;
```

### 定义枚举类型的同时定义枚举变量
```
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
} day;
```

### 省略枚举名称，直接定义枚举变量
```c
enum
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
} day;
```

### 实例:
```c
#include <stdio.h>
 
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
 
int main()
{
    enum DAY day;
    day = WED;
    printf("%d",day);
    return 0;
}
```
以上实例输出结果为：
```
3
```
在C 语言中，枚举类型是被当做 int 或者 unsigned int 类型来处理的，所以按照 C 语言规范是没有办法遍历枚举类型的。
不过在一些特殊的情况下，枚举类型必须连续是可以实现有条件的遍历。
以下实例使用 for 来遍历枚举的元素：
```c
#include <stdio.h>
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
} day;
int main()
{
    // 遍历枚举元素
    for (day = MON; day <= SUN; day++) {
        printf("枚举元素：%d \n", day);
    }
}
```
以上实例输出结果为：
```
枚举元素：1 
枚举元素：2 
枚举元素：3 
枚举元素：4 
枚举元素：5 
枚举元素：6 
枚举元素：7
```
