# C语言程序设计 Part-4


C头文件,C强制类型转换,C错误处理,C可变参数
<!--more-->

# C头文件
头文件是扩展名为 .h 的文件，包含了 C 函数声明和宏定义，被多个源文件中引用共享。有两种类型的头文件：程序员编写的头文件和编译器自带的头文件。
在程序中要使用头文件，需要使用 C 预处理指令 #include 来引用它。前面我们已经看过 stdio.h 头文件，它是编译器自带的头文件。
引用头文件相当于**复制头文件的内容**
使用预处理指令 #include 可以引用用户和系统头文件。它的形式有以下两种：
```c
#include <file>
```
这种形式用于**引用系统头文件**。它在系统目录的标准列表中搜索名为 file 的文件。在编译源代码时，您可以通过 -I 选项把目录前置在该列表前。
```c
#include "file"
```
这种形式用于**引用用户头文件**。它在包含当前文件的目录中搜索名为 file 的文件。在编译源代码时，您可以通过 -I 选项把目录前置在该列表前。

## 引用头文件的操作
#include 指令会指示 C 预处理器浏览指定的文件作为输入。预处理器的输出包含了已经生成的输出，被引用文件生成的输出以及 #include 指令之后的文本输出。例如，如果您有一个头文件 header.h，如下：
```
char *test (void);
```
和一个使用了头文件的主程序 program.c，如下：
```c
int x;
#include "header.h"

int main (void)
{
   puts (test ());
}
```
编译器会看到如下的代码信息：
```
int x;
char *test (void);

int main (void)
{
   puts (test ());
}
```

## 只引用一次头文件
如果一个头文件被引用两次，编译器会处理两次头文件的内容，这将产生错误。为了防止这种情况，标准的做法是把文件的整个内容放在条件编译语句中，如下：
```c
#ifndef HEADER_FILE
#define HEADER_FILE
#endif
```
这种结构就是通常所说的包装器 #ifndef。当再次引用头文件时，条件为假，因为 HEADER_FILE 已定义。此时，预处理器会跳过文件的整个内容，编译器会忽略它。

## 有条件引用
有时需要从多个不同的头文件中选择一个引用到程序中。例如，**需要指定在不同的操作系统上使用的配置参数**。您可以通过一系列条件来实现这点，如下：
```c
#if SYSTEM_1
   # include "system_1.h"
#elif SYSTEM_2
   # include "system_2.h"
#elif SYSTEM_3
   ...
#endif
```

# C强制类型转换
```c
(type_name) expression
```
```c
#include <stdio.h>
int main()
{
   int sum = 17, count = 5;
   double mean;
   mean = (double) sum / count;
   printf("Value of mean : %f\n", mean );
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
Value of mean : 3.400000
```
这里要注意的是**强制类型转换运算符的优先级大于除法**，因此 sum 的值首先被转换为 double 型，然后除以 count，得到一个类型为 double 的值。
类型转换可以是隐式的，由编译器自动执行，也可以是显式的，通过使用强制类型转换运算符来指定。在编程时，**有需要类型转换的时候都用上强制类型转换运算符，是一种良好的编程习惯**。

## 整数提升
整数提升是指把小于 int 或 unsigned int 的整数类型转换为 int 或 unsigned int 的过程。请看下面的实例，在 int 中添加一个字符：
```c
#include <stdio.h>
int main()
{
   int  i = 17;
   char c = 'c'; /* ascii 值是 99 */
   int sum;
   sum = i + c;
   printf("Value of sum : %d\n", sum );
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
Value of sum : 116
```
在这里，sum 的值为 116，因为编译器进行了整数提升，**在执行实际加法运算时，把 'c' 的值转换为对应的 ascii 值**。

# C错误处理
C 语言不提供对错误处理的直接支持，但是作为一种系统编程语言，它以**返回值的形式**允许您访问底层数据。在**发生错误时，大多数的 C 或 UNIX 函数调用返回 1 或 NULL**，同时会设置一个错误代码 errno，该错误代码是全局变量，表示在函数调用期间发生了错误。您可以在 **errno.h** 头文件中找到各种各样的错误代码。
所以，C 程序员可以通过检查返回值，然后根据返回值决定采取哪种适当的动作。开发人员应该在程序初始化时，**把 errno 设置为 0，这是一种良好的编程习惯**。0 值表示程序中没有错误。

## errno、perror() 和 strerror()
C 语言提供了 perror() 和 strerror() 函数来显示与 errno 相关的文本消息。
 - perror() 函数显示您传给它的**字符串，后跟一个冒号、一个空格和当前 errno 值的文本表示形式**。
 - strerror() 函数，返回一个指针，指针指向**当前 errno 值的文本表示形式**。
让我们来模拟一种错误情况，尝试打开一个不存在的文件。您可以使用多种方式来输出错误消息，在这里我们使用函数来演示用法。另外有一点需要注意，您应该使用**stderr 文件流来输出所有的错误**。
```c
#include <stdio.h>
#include <errno.h>
#include <string.h>
extern int errno ;
int main ()
{
   FILE * pf;
   int errnum;
   pf = fopen ("unexist.txt", "rb");
   if (pf == NULL)
   {
      errnum = errno;
      fprintf(stderr, "错误号: %d\n", errno);
      perror("通过 perror 输出错误");
      fprintf(stderr, "打开文件错误: %s\n", strerror( errnum ));
   }
   else
   {
      fclose (pf);
   }
   return 0;
}
```

### sprintf、fprintf和printf
都是把格式好的字符串输出，但是输出的目标不一样：
1. printf就是标准输出，在屏幕上打印出一段字符串来。
2. sprintf就是把格式化的数据写入到某个字符串中。返回值字符串的长度。
3. fprintf是用于文件操作。
原型：int fprintf(FILE *stream,char *format,[argument])；       
功能：fprintf()函数根据指定的format(格式)发送信息(参数)到由stream(流)指定的文件.因此fprintf()可以使得信息输出到指定的文件。

### stderr和stdout的区别
stdout -- 标准输出设备。
stderr -- 标准错误输出设备 
两者默认向屏幕输出,但如果用转向标准输出到磁盘文件，stdout输出到磁盘文件，stderr在屏幕。 
**stdout是行缓冲的，他的输出会放在一个buffer里面，只有到换行的时候，才会输出到屏幕。而stderr是无缓冲的，会直接输出**
```c
int main(){
fprintf(stdout,"Hello ");
fprintf(stderr,"World!");
return0;
}
```
输出结果为:
```c
World!Hello
```
printf(stdout, "xxxx") 和 printf(stdout, "xxxx\n")，前者会憋住，直到遇到新行才会一起输出。而printf(stderr, "xxxxx")，不管有么有'\n'，都输出。

### exit()和return
 - exit（0）：正常运行程序并退出程序；
 - exit（1）：非正常运行导致退出程序；
 - return（）：返回函数，若在主函数中，则会退出函数并返回一值。
1. return返回函数值，是关键字;exit 是一个函数。
2. return是**C语言级别**的，它表示了调用堆栈的返回；而exit是**系统调用级别**的，它表示了一个进程的结束。
3. return是函数的退出(返回)；exit是进程的退出。
4. return是C语言提供的，exit是操作系统提供的（或者函数库中给出的）。
5. return用于结束一个函数的执行，将函数的执行信息传出个其他调用函数使用；exit函数是退出应用程序，删除进程使用的内存空间，并将应用程序的一个状态返回给OS，这个状态标识了应用程序的一些运行信息，这个信息和机器和操作系统有关，一般是 0 为正常退出，非0 为非正常退出。
6. 非主函数中调用return和exit效果很明显，但是在main函数中调用return和exit的现象就很模糊，多数情况下现象都是一致的。

## 程序退出状态
通常情况下，程序成功执行完一个操作正常退出的时候会带有值 EXIT_SUCCESS。在这里，**EXIT_SUCCESS 是宏**，它被定义为 0。
如果程序中存在一种错误情况，当您退出程序时，会带有状态值 EXIT_FAILURE，被定义为 -1。
(个人认为这是一种增加代码可读性的行为)
上面的程序可以写成：
```c
#include <stdio.h>
#include <stdlib.h>
main()
{
   int dividend = 20;
   int divisor = 5;
   int quotient;
   if( divisor == 0){
      fprintf(stderr, "除数为 0 退出运行...\n");
      exit(EXIT_FAILURE);
   }
   quotient = dividend / divisor;
   fprintf(stderr, "quotient 变量的值为: %d\n", quotient );
   exit(EXIT_SUCCESS);
}
```

# C可变参数
```c
int func(int, ... ) 
{
   ...
}
int main()
{
   func(2, 2, 3);
   func(3, 2, 3, 4);
}
```
函数 func() 最后一个参数写成省略号，即三个点号（...），省略号之前的那个参数是 int，代表了要传递的可变参数的总数。为了使用这个功能，您需要使用**stdarg.h头文件**，该文件提供了实现可变参数功能的函数和宏。具体步骤如下：
 - 定义一个函数，最后一个参数为省略号，省略号前面可以设置自定义参数。
 - 在函数定义中创建一个**va_list 类型变量**，该类型是在 stdarg.h 头文件中定义的。
 - 使用 int 参数和 **va_start 宏来初始化** va_list 变量为一个参数列表。宏 va_start 是 在 stdarg.h 头文件中定义的。
 - 使用 **va_arg 宏和 va_list 变量**来**访问参数列表中的每个项**。
 - 使用宏 **va_end 清理**赋予 va_list 变量的内存。

1. va_start宏，获取可变参数列表的第一个参数的地址（list是类型为va_list的指针，param1是可变参数最左边的参数）：
```
#define va_start(list,param1)   ( list = (va_list)&param1+ sizeof(param1) )
```
2. va_arg宏，获取可变参数的当前参数，返回指定类型并将指针指向下一参数（**mode参数描述了当前参数的类型**）：
```
#define va_arg(list,mode)   ( (mode *) ( list += sizeof(mode) ) )[-1]
```
3. va_end宏，清空va_list可变参数列表：
```
#define va_end(list) ( list = (va_list)0 )
```

## 实例
### 传入int
```c
#include <stdio.h>
#include <stdarg.h>
 
double average(int num,...)
{
    va_list valist;
    double sum = 0.0;
    int i;
    /* 为 num 个参数初始化 valist */
    va_start(valist, num);
 
    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
       sum += va_arg(valist, int);
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);
    return sum/num;
}
int main()
{
   printf("Average of 2, 3, 4, 5 = %f\n", average(4, 2,3,4,5));
   printf("Average of 5, 10, 15 = %f\n", average(3, 5,10,15));
}
```
输出以下结果
```c
Average of 2, 3, 4, 5 = 3.500000
Average of 5, 10, 15 = 10.000000
```

### 传入字符串
```c
#include <stdio.h>
#include <stdarg.h>

void var_test(char *format, ...)
{
    va_list list;
    va_start(list,format);
    
    char *ch;
    while(1)
    {
         ch = va_arg(list, char *);
         if(strcmp(ch,"") == 0)
         {    
               printf("\n");
               break;
         }
         printf("%s ",ch);
     }
     va_end(list);
}
 
int main()
{
    var_test("test","this","is","a","test","");
    return 0;
}
```

## 可变参数应用实例
printf实现
```c
#include <stdarg.h>
int printf(char *format, ...)
{
    va_list ap;
    int n;
    va_start(ap, format);
    n = vprintf(format, ap);
    va_end(ap);
    return n;    
}
```







