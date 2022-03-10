# C语言程序设计 Part-2


指针,字符串,结构体,位域,共用体
<!--more-->

# C 中的 NULL 指针
在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 NULL 值是一个良好的编程习惯。**赋为 NULL 值的指针被称为空指针**。
NULL 指针是一个定义在标准库中的值为零的常量。请看下面的程序：
```c
#include <stdio.h>
int main ()
{
   int  *ptr = NULL;
   printf("ptr 的地址是 %p\n", ptr  );
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
ptr 的地址是 0x0
```
在大多数的操作系统上，程序不允许访问地址为 0 的内存，因为该内存是操作系统保留的。然而，内存地址 0 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。但按照惯例，如果指针包含空值（零值），则假定它不指向任何东西。
如需检查一个空指针，您可以使用 if 语句，如下所示：
```c
if(ptr)     /* 如果 p 非空，则完成 */
if(!ptr)    /* 如果 p 为空，则完成 */
```

# 函数指针
函数指针是指向函数的指针变量。
通常我们说的指针变量是指向一个整型、字符型或数组等变量，而函数指针是指向函数。
函数指针可以像一般函数一样，用于调用函数、传递参数。
函数指针变量的声明：
```c
typedef int (*fun_ptr)(int,int); // 声明一个指向同样参数、返回值的函数指针类型
```
实例
```c
#include <stdio.h>
int max(int x, int y)
{
    return x > y ? x : y;
}
int main(void)
{
    /* p 是函数指针 */
    int (* p)(int, int) = & max; // &可以省略
    int a, b, c, d;
    printf("请输入三个数字:");
    scanf("%d %d %d", & a, & b, & c);
    /* 与直接调用函数等价，d = max(max(a, b), c) */
    d = p(p(a, b), c); 
    printf("最大的数字是: %d\n", d);
    return 0;
}
```
输出结果:
```c
请输入三个数字:1 2 3
最大的数字是: 3
```

# 回调函数
函数指针作为某个函数的参数
函数指针变量可以作为某个函数的参数来使用的，回调函数就是一个通过函数指针调用的函数。
简单讲：回调函数是由别人的函数执行时调用你实现的函数。

1. 通俗理解
>你到一个商店买东西，刚好你要的东西没有货，于是你在店员那里留下了你的电话，过了几天店里有货了，店员就打了你的电话，然后你接到电话后就到店里去取了货。在这个例子里，你的电话号码就叫回调函数，你把电话留给店员就叫登记回调函数，店里后来有货了叫做触发了回调关联的事件，店员给你打电话叫做调用回调函数，你到店里去取货叫做响应回调事件。 

2. 官方定义:
>回调函数就是一个通过函数指针调用的函数。如果你把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时，我们就说这是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

3. 形象解释:
>比如，我们写A B C D 四个函数，封装成一个库文件，然后我们的主函数里面要写一个功能函数，这个功能要用到函数A，假如不用函数指针，这个功能函数就要调用函数A，下次如果用到函数B，那么我们得删掉A，调用函数B，每次都要修改这个函数很麻烦，但如果使用回调函数就不一样了，我们可以定义4个函数指针，把4个函数的地址分别赋给4个函数指针，然后将函数指针当作参数传递给功能函数，功能函数就可以通过修改参数来调用对应的函数，而它本身不用做任何的修改。**这样的话，功能函数就可以根据不同的情况，通过函数指针去调用不同的函数**，代码如下
```c
#include <stdio.h>
#include <stdlib.h>
float ADD(float a, float b)
{
    return a + b;
}

float SUB(float a, float b)
{
    return a - b;
}

float MUL(float a, float b)
{
    return a * b;
}

float DIV(float a, float b)
{
    return a / b;
}

float (*A)(float x, float y) = ADD;
float (*B)(float x, float y) = SUB;
float (*C)(float x, float y) = MUL;
float (*D)(float x, float y) = DIV;

float  fun(float x, float y, float(*p)(float x, float y)) {
     return p(x, y);
}

int main()
{
    printf("%f", fun(2, 3, A));

}
```

# C 字符串
在 C 语言中，字符串实际上是**使用 null 字符 '\0' 终止的一维字符数组**。因此，一个以 null 结尾的字符串，包含了组成字符串的字符。
## C 中操作字符串的函数
1. **strcpy(s1, s2);**
复制字符串 s2 到字符串 s1。
2. **strcat(s1, s2);**
连接字符串 s2 到字符串 s1 的末尾。
3. **strlen(s1);**
返回字符串 s1 的长度。
4. **strcmp(s1, s2);**
如果 s1 和 s2 是相同的，则返回 0；如果 s1<s2 则返回小于 0；如果 s1>s2 则返回大于 0。
5. **strchr(s1, ch);**
返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。
6. **strstr(s1, s2);**
返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置。

# C 结构体
## 定义结构
为了定义结构，您必须使用 struct 语句。struct 语句定义了一个包含多个成员的新的数据类型，struct 语句的格式如下：
```c
struct tag { 
    member-list
    member-list 
    member-list  
    ...
} variable-list ;
```
1. tag 是结构体标签。
2. member-list 是标准的变量定义，比如 int i; 或者 float f，或者其他有效的变量定义。
3. variable-list **结构变量**，定义在结构的末尾，最后一个分号之前，您可以指定一个或多个结构变量。
声明的结构变量,即已经实例化的变量

## 结构体变量的初始化
和其它类型变量一样，对结构体变量可以在定义时指定初始值
```c
#include <stdio.h>
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
} book = {"C 语言", "RUNOOB", "编程语言", 123456};
int main()
{
    printf("title : %s\nauthor: %s\nsubject: %s\nbook_id: %d\n", book.title, book.author, book.subject, book.book_id);
}
```

# C位域
有些信息在存储时，并不需要占用一个完整的字节，而只需占几个或一个二进制位。例如在存放一个开关量时，只有 0 和 1 两种状态，用 1 位二进位即可。为了节省存储空间，并使处理简便，C 语言又提供了一种数据结构，称为"位域"或"位段"。
所谓"位域"是**把一个字节中的二进位划分为几个不同的区域**，并说明每个区域的位数。每个域有一个域名，允许在程序中按域名进行操作。这样就可以把几个不同的对象用一个字节的二进制位域来表示。

典型的实例：
 - 用 1 位二进位存放一个开关量时，只有 0 和 1 两种状态。
 - 读取外部文件格式——可以读取非标准的文件格式。例如：9 位的整数。

## 位域的定义和位域变量的说明
在结构内声明位域的形式如下：
```c
struct
{
  type [member_name] : width ;
};
```
 - type只能为 int(整型)，unsigned int(无符号整型)，signed int(有符号整型) 三种类型，决定了如何解释位域的值。
 - member_name 位域的名称。
 - width 位域中位的数量。宽度必须小于或等于指定类型的位宽度。

带有预定义宽度的变量被称为位域。位域可以存储多于 1 位的数，例如，需要一个变量来存储从**0到7**的值，您可以定义一个宽度为 3 位的位域，如下：
```c
struct
{
  unsigned int age : 3;
} Age;
```
上面的结构定义指示 C 编译器，**age 变量将只使用 3 位来存储这个值**，如果您试图使用超过 3 位，则无法完成
```c
include <stdio.h>
#include <string.h>
struct
{
  unsigned int age : 3;
} Age;

int main( )
{
   Age.age = 4;
   printf( "Sizeof( Age ) : %d\n", sizeof(Age) );
   printf( "Age.age : %d\n", Age.age );
   Age.age = 7;
   printf( "Age.age : %d\n", Age.age );
   Age.age = 8; // 二进制表示为 1000 有四位，超出
   printf( "Age.age : %d\n", Age.age );
   return 0;
}
```
当上面的代码被编译时，它会带有警告，当上面的代码被执行时，它会产生下列结果：
```c
Sizeof( Age ) : 4
Age.age : 4
Age.age : 7
Age.age : 0
```

### 实例:
```c
struct bs{
    int a:8;
    int b:2;
    int c:6;
}data;
```
说明 data 为 bs 变量，共占两个字节。其中位域 a 占 8 位，位域 b 占 2 位，位域 c 占 6 位。

### 对于位域的定义尚有以下几点说明：
一个位域存储在同一个字节中，如**一个字节所剩空间不够存放另一位域时，则会从下一单元起存放该位域**。也可以有意使某位域从下一单元开始。例如：
```c
struct bs{
    unsigned a:4;
    unsigned  :4;    /* 空域 */
    unsigned b:4;    /* 从下一单元开始存放 */
    unsigned c:4
}
```
在这个位域定义中，a 占第一字节的 4 位，后 4 位填 0 表示不使用，b 从第二字节开始，占用 4 位，c 占用 4 位。
由于位域不允许跨两个字节，因此位域的长度不能大于一个字节的长度，也就是说不能超过8位二进位。如果最大长度大于计算机的整数字长，一些编译器可能会允许域的内存重叠，另外一些编译器可能会把大于一个域的部分存储在下一个字中。
位域可以是**无名位域**，这时它只用来作**填充或调整位置,无名的位域是不能使用的**,例如：
```c
struct k{
    int a:1;
    int  :2;    /* 该 2 位不能使用 */
    int b:3;
    int c:2;
};
```
从以上分析可以看出，位域在本质上就是一种结构类型，不过其成员是按二进位分配的。
```c
#include <stdio.h>
#include <string.h>
/* 定义简单的结构 */
struct
{
  unsigned int widthValidated;
  unsigned int heightValidated;
} status1;
 
/* 定义位域结构 */
struct
{
  unsigned int widthValidated : 1;
  unsigned int heightValidated : 1;
} status2;
 
int main( )
{
   printf( "Memory size occupied by status1 : %d\n", sizeof(status1));
   printf( "Memory size occupied by status2 : %d\n", sizeof(status2));
 
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
Memory size occupied by status1 : 8
Memory size occupied by status2 : 4
```

# C 共用体
共用体是一种特殊的数据类型，允许您在**相同的内存位置存储不同的数据类型**。您可以定义一个带有多成员的共用体，但是任何时候只**能有一个成员带有值**。共用体提供了一种使用相同的内存位置的有效方式。
## 定义共用体
为了定义共用体，您必须使用 union 语句，方式与定义结构类似。union 语句定义了一个新的数据类型，带有多个成员。union 语句的格式如下：
```c
union [union tag]
{
   member definition;
   member definition;
   ...
   member definition;
} [one or more union variables];
```
union tag 是可选的，每个 member definition 是标准的变量定义，比如 int i; 或者 float f; 或者其他有效的变量定义。在共用体定义的末尾，最后一个分号之前，您可以指定一个或多个共用体变量，这是可选的。下面定义一个名为 Data 的共用体类型，有三个成员 i、f 和 str：
```c
union Data
{
   int i;
   float f;
   char  str[20];
} data;
```
现在，Data 类型的变量可以存储一个整数、一个浮点数，或者一个字符串。这意味着一个变量（相同的内存位置）可以存储多个多种类型的数据。您可以根据需要在一个共用体内使用任何内置的或者用户自定义的数据类型。
**共用体占用的内存应足够存储共用体中最大的成员**。例如，在上面的实例中，**Data 将占用 20 个字节的内存空间**，因为在各个成员中，字符串所占用的空间是最大的。

## 访问共用体成员
为了访问共用体的成员，我们使用成员访问运算符（.）。成员访问运算符是共用体变量名称和我们要访问的共用体成员之间的一个句号。您可以使用 union 关键字来定义共用体类型的变量。下面的实例演示了共用体的用法：
```c
#include <stdio.h>
#include <string.h>
union Data
{
   int i;
   float f;
   char  str[20];
};
int main( )
{
   union Data data;        
   data.i = 10;
   printf( "data.i : %d\n", data.i);
   data.f = 220.5;
   printf( "data.f : %f\n", data.f);
   strcpy( data.str, "C Programming");
   printf( "data.str : %s\n", data.str);
   return 0;
}
```
产生以下结果
```
data.i : 10
data.f : 220.500000
data.str : C Programming
```


