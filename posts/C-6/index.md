# C语言程序设计 Part-3


typedef与#define,C输入&输出,C文件读写,C预处理器
<!--more-->

# typedef 与 #define
 - typedef 仅限于为类型定义符号名称，#define 不仅可以为类型定义别名，也能为数值定义别名，比如您可以定义 1 为 ONE。
 - typedef 是由编译器执行解释的，#define 语句是由预编译器进行处理的。
```c
#include <stdio.h>
#define TRUE  1
#define FALSE 0
int main( )
{
   printf( "TRUE 的值: %d\n", TRUE);
   printf( "FALSE 的值: %d\n", FALSE);
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
TRUE 的值: 1
FALSE 的值: 0
```

# C 输入 & 输出
值得注意的是:
**%f和%lf**分别是**float类型和double类型**用于**格式化输入输出时对应的格式符号**。
其中：
 - float，单精度浮点型，对应%f。
 - double,双精度浮点型，对应%lf。

## getchar() & putchar() 函数
 - int getchar(void) 函数
  从屏幕读取下一个可用的字符，并把它返回为一个整数。这个函数在同一个时间内只会读取一个单一的字符。您可以在循环内使用这个方法，以便从屏幕上读取多个字符。
 - int putchar(int c) 函数
  把字符输出到屏幕上，并返回相同的字符。这个函数在同一个时间内只会输出一个单一的字符。您可以在循环内使用这个方法，以便在屏幕上输出多个字符。

## ets() & puts() 函数
 - char *gets(char *s) 函数
  从 stdin 读取一行到 s 所指向的缓冲区，直到一个终止符或EOF。
 - int puts(const char *s) 函数
  把字符串 s 和一个尾随的换行符写入到 stdout。
```c
#include <stdio.h>
int main( )
{
   char str[100];
 
   printf( "Enter a value :");
   gets( str );
 
   printf( "\nYou entered: ");
   puts( str );
   return 0;
}
```
当上面的代码被编译和执行时，它会等待您输入一些文本，当您输入一个文本并**按下回车键时**，程序会继续并读取一整行直到该行结束，显示如下：
```c
$./a.out
Enter a value :runoob
You entered: runoob
```

# C 文件读写
## 打开文件
您可以使用 fopen( ) 函数来创建一个新的文件或者打开一个已有的文件，这个调用会初始化类型 FILE 的一个对象，类型 FILE 包含了所有用来控制流的必要的信息。
```c
FILE *fopen( const char * filename, const char * mode );
```
filename 是字符串，用来命名文件，访问模式 mode 的值可以是下列值中的一个：
1. r 打开一个已有的文本文件，允许读取文件。
2. w 打开一个文本文件，允许写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会从文件的开头写入内容。如果文件存在，则该会被截断为零长度，重新写入。
3. a 打开一个文本文件，以追加模式写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会在已有的文件内容中追加内容。
4. r+ 打开一个文本文件，允许读写文件。
5. w+ 打开一个文本文件，允许读写文件。如果文件已存在，则文件会被截断为零长度，如果文件不存在，则会创建一个新文件。
6. a+ 打开一个文本文件，允许读写文件。如果文件不存在，则会创建一个新文件。读取会从文件的开头开始，写入则只能是追加模式。

如果**处理的是二进制文件**，则需使用下面的访问模式来取代上面的访问模式：
```c
"rb", "wb", "ab", "rb+", "r+b", "wb+", "w+b", "ab+", "a+b"
```

## 关闭文件
为了关闭文件，请使用 fclose( ) 函数。函数的原型如下：
```c
 int fclose( FILE *fp );
```
如果成功关闭文件，fclose( ) 函数返回零，如果关闭文件时发生错误，函数返回 EOF。这个函数实际上，会清空缓冲区中的数据，关闭文件，并释放用于该文件的所有内存。EOF 是一个定义在头文件 stdio.h 中的常量。
C 标准库提供了各种函数来按字符或者以固定长度字符串的形式读写文件。

## 写入文件
下面是把字符写入到流中的最简单的函数：
```c
int fputc( int c, FILE *fp );
```
函数 fputc() 把**参数c的字符**值写入到 fp 所指向的输出流中。如果写入成功，它会返回写入的字符，如果发生错误，则会返回 EOF。您可以使用下面的函数来把一个以 null 结尾的字符串写入到流中：
```c
int fputs( const char *s, FILE *fp );
```
函数 fputs() 把**字符串s**写入到 fp 所指向的输出流中。如果写入成功，它会返回一个非负值，如果发生错误，则会返回 EOF。您也可以使用 int fprintf(FILE *fp,const char *format, ...) 函数来写把一个字符串写入到文件中

### 实例
```c
#include <stdio.h>
int main()
{
   FILE *fp = NULL;
   fp = fopen("/tmp/test.txt", "w+");
   fprintf(fp, "This is testing for fprintf...\n");
   fputs("This is testing for fputs...\n", fp);
   fclose(fp);
}
``` 

## 读取文件
下面是从文件读取单个字符的最简单的函数：
```c
int fgetc( FILE * fp );
```
fgetc() 函数从 fp 所指向的输入文件中**读取一个字符**。返回值是读取的字符，如果发生错误则返回 EOF。
```c
char *fgets( char *buf, int n, FILE *fp );
```
函数 fgets() 从 fp 所指向的输入流中**读取 n - 1 个字符**。它会把读取的字符串复制到缓冲区 buf，并在最后追加一个 null 字符来终止字符串。
如果这个函数在读取最后一个字符之前就遇到一个换行符 '\n' 或文件的末尾 EOF，则只会返回读取到的字符，包括换行符。
int fscanf(FILE *fp, const char *format, ...) 函数来从文件中读取字符串，但是在遇到第一个空格和换行符时，它会停止读取。

### 实例
```c
#include <stdio.h>
int main()
{
   FILE *fp = NULL;
   char buff[255];
   fp = fopen("/tmp/test.txt", "r");
   fscanf(fp, "%s", buff);
   printf("1: %s\n", buff );
   fgets(buff, 255, (FILE*)fp);
   printf("2: %s\n", buff ); 
   fgets(buff, 255, (FILE*)fp);
   printf("3: %s\n", buff );
   fclose(fp);
}
```
首先，fscanf() 方法只读取了 This，因为它在后边遇到了一个空格(遇到空格停止读取)。其次，调用 fgets() 读取剩余的部分，直到行尾。(遇到换行符结束读取)
最后调用 fgets() 完整地读取第二行。

## EOF的定义
EOF为End Of File的缩写，在操作系统中表示资料源无更多的资料可读取。资料源通常称为档案或串流。**通常在文本的最后存在此字符表示资料结束**。
从一个终端的输入从来不会真的“结束”（除非设备被断开），但把从终端输入的数据分区成多个“文件”却很有用，因此一个关键的序列被保留下来来指明输入结束。
 - 在UNIX和AmigaDOS中，将击键翻译为EOF的过程是由终端的驱动程序完成的，因此应用程序无需将终端和其它输入文件区分开来。Unix平台的驱动程序在行首传送一个传输结束字符（**Control-D**，ASCII编码为为04）来指明文件结束。
 - 在微软的DOS和Windows（以及CP/M和许多DEC操作系统）中，读取数据时终端不会产生EOF。此时，应用程序知道数据源是一个终端（或者其它“字符设备”），并将一个已知的保留的字符或序列解释为文件结束的指明；最普遍地说，它是ASCII码中的替换字符（**Control-Z**，代码26）

# C 预处理器
C 预处理器不是编译器的组成部分，但是它是编译过程中一个单独的步骤。简言之，C 预处理器只不过是一个**文本替换工具**而已，它们会指示编译器在实际编译之前完成所需的预处理。我们将把 C 预处理器（C Preprocessor）简写为 CPP(然而我并没有看懂)。
所有的预处理器命令都是以井号（#）开头。它必须是第一个非空字符，为了增强可读性，预处理器指令应从第一列开始。下面列出了所有重要的预处理器指令：
 - #define	定义宏
 - #include	包含一个源代码文件
 - #undef	取消已定义的宏
 - #ifdef	如果宏已经定义，则返回真
 - #ifndef	如果宏没有定义，则返回真
 - #if	如果给定条件为真，则编译下面代码
 - #else	#if 的替代方案
 - #elif	如果前面的 #if 给定条件不为真，当前条件为真，则编译下面代码
 - #endif	结束一个 #if……#else 条件编译块
 - #error	当遇到标准错误时，输出错误消息
 - #pragma	使用标准化方法，向编译器发布特殊的命令到编译器中

## 预处理器实例
```c
#define MAX_ARRAY_LENGTH 20
```
这个指令告诉 CPP 把所有的 MAX_ARRAY_LENGTH 替换为 20。使用 #define 定义常量来增强可读性。
```c
#include <stdio.h>
#include "myheader.h"
```
这些指令告诉 CPP 从系统库中获取 stdio.h，并添加文本到当前的源文件中。下一行告诉 CPP 从本地目录中获取 myheader.h，并添加内容到当前的源文件中。
```c
#undef  FILE_SIZE
#define FILE_SIZE 42
```
这个指令告诉 CPP 取消已定义的 FILE_SIZE，并定义它为 42。
```c
#ifndef MESSAGE
   #define MESSAGE "You wish!"
#endif
```
这个指令告诉 CPP 只有当 MESSAGE 未定义时，才定义 MESSAGE。
```c
#ifdef DEBUG
   /* Your debugging statements here */
#endif
```
这个指令告诉 CPP 如果定义了 DEBUG，则执行处理语句。在编译时，如果您向 gcc 编译器传递了 -DDEBUG 开关量，这个指令就非常有用。它定义了 DEBUG，您可以在编译期间随时开启或关闭调试。

## 预定义宏
ANSI C 定义了许多宏。在编程中您**可以使用这些宏，但是不能直接修改这些预定义的宏**。
 - __DATE__	当前日期，一个以 "MMM DD YYYY" 格式表示的字符常量。
 - __TIME__	当前时间，一个以 "HH:MM:SS" 格式表示的字符常量。
 - __FILE__	这会包含当前文件名，一个字符串常量。
 - __LINE__	这会包含当前行号，一个十进制常量。
 - __STDC__	当编译器以 ANSI 标准编译时，则定义为 1。

### 实例
```c
#include <stdio.h>
main()
{
   printf("File :%s\n", __FILE__ );
   printf("Date :%s\n", __DATE__ );
   printf("Time :%s\n", __TIME__ );
   printf("Line :%d\n", __LINE__ );
   printf("ANSI :%d\n", __STDC__ );

}
```
当上面的代码（在文件 test.c 中）被编译和执行时，它会产生下列结果：
```c
File :test.c
Date :Jun 2 2012
Time :03:36:24
Line :8
ANSI :1
```

## 预处理器运算符
### 宏延续运算符（\）
一个宏通常写在一个单行上。但是如果宏太长，一个单行容纳不下，则使用宏延续运算符（\）。例如：
```c
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")
```

### 字符串常量化运算符（#）
在宏定义中，当需要把一个宏的参数转换为字符串常量时，则使用字符串常量化运算符（#）。在宏中使用的该运算符有一个特定的参数或参数列表。例如：
```c
#include <stdio.h>
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")

int main(void)
{
   message_for(Carole, Debra);
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
Carole and Debra: We love you!
```

### 标记粘贴运算符（##）
宏定义内的标记粘贴运算符（##）会合并两个参数。它允许在宏定义中两个独立的标记被合并为一个标记。例如：
```c
#include <stdio.h>
#define tokenpaster(n) printf ("token" #n " = %d", token##n)

int main(void)
{
   int token34 = 40;
   
   tokenpaster(34);
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
token34 = 40
```
这是怎么发生的，因为这个实例会从编译器产生下列的实际输出：
```c
printf ("token34 = %d", token34);
```
这个实例演示了 token##n 会连接到 token34 中，在这里，我们使用了字符串常量化运算符（#）和标记粘贴运算符（##）。

### defined() 运算符
预处理器 defined 运算符是用在常量表达式中的，用来确定一个标识符是否已经使用 #define 定义过。如果指定的标识符已定义，则值为真（非零）。如果指定的标识符未定义，则值为假（零）。下面的实例演示了 defined() 运算符的用法：
```c
#include <stdio.h>
#if !defined (MESSAGE)
   #define MESSAGE "You wish!"
#endif

int main(void)
{
   printf("Here is the message: %s\n", MESSAGE);  
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
Here is the message: You wish!
```

## 参数化的宏
CPP 一个强大的功能是可以使用参数化的宏来模拟函数。例如，下面的代码是计算一个数的平方：
```c
int square(int x) {
   return x * x;
}
```
我们可以使用宏重写上面的代码，如下：
```c
#define square(x) ((x) * (x))
```
在使用带有参数的宏之前，必须使用 #define 指令定义。参数列表是括在圆括号内，且必须紧跟在宏名称的后边。宏名称和左圆括号之间不允许有空格。例如：
```c
#include <stdio.h>
#define MAX(x,y) ((x) > (y) ? (x) : (y))

int main(void)
{
   printf("Max between 20 and 10 is %d\n", MAX(10, 20));  
   return 0;
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```
Max between 20 and 10 is 20
```
