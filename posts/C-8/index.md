# C语言程序设计 Part-5


C内存管理,C命令行参数
<!--more-->

# C内存管理
1. void *calloc(int num, int size);
在内存中动态地分配 num 个长度为 size 的连续空间，并将每一个字节都初始化为 0。所以它的结果是分配了 num*size 个字节长度的内存空间，并且每个字节的值都是0。
2. void free(void *address);
该函数释放 address 所指向的内存块,释放的是动态分配的内存空间。
3. void *malloc(int num);
在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后不会被初始化，它们的值是未知的。
4. void *realloc(void *address, int newsize);
该函数重新分配内存，把内存扩展到 newsize。

void * 类型表示未确定类型的指针。C、C++ 规定 void * 类型可以通过类型转换强制转换为任何其它类型的指针。

## 重新调整内存的大小和释放内存
当程序退出时，操作系统会自动释放所有分配给程序的内存，但是，建议您在不需要内存时，都应该调用函数 free() 来释放内存。
或者，您可以通过调用函数 realloc() 来增加或减少已分配的内存块的大小。让我们使用 realloc() 和 free() 函数，再次查看上面的实例：
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
int main()
{
   char name[100];
   char *description;
   strcpy(name, "Zara Ali");
   /* 动态分配内存 */
   description = (char *)malloc( 30 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcpy( description, "Zara ali a DPS student.");
   }
   /* 假设您想要存储更大的描述信息 */
   description = (char *) realloc( description, 100 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcat( description, "She is in class 10th");
   }
   printf("Name = %s\n", name );
   printf("Description: %s\n", description );
   /* 使用 free() 函数释放内存 */
   free(description);
}
```
当上面的代码被编译和执行时，它会产生下列结果：
```c
Name = Zara Ali
Description: Zara ali a DPS student.She is in class 10th
```

# C命令行参数
执行程序时，可以从命令行传值给 C 程序。这些值被称为命令行参数，它们对程序很重要，特别是当您想从外部控制程序，而不是在代码内对这些值进行硬编码时，就显得尤为重要了。
命令行参数是使用 main() 函数参数来处理的，其中，**argc 是指传入参数的个数，argv[] 是一个指针数组，指向传递给程序的每个参数**。下面是一个简单的实例，检查命令行是否有提供参数，并根据参数执行相应的动作：
```c
#include <stdio.h>

int main( int argc, char *argv[] )  
{
   if( argc == 2 )
   {
      printf("The argument supplied is %s\n", argv[1]);
   }
   else if( argc > 2 )
   {
      printf("Too many arguments supplied.\n");
   }
   else
   {
      printf("One argument expected.\n");
   }
}
```
使用一个参数，编译并执行上面的代码，它会产生下列结果：
```c
$./a.out testing
The argument supplied is testing
```
使用两个参数，编译并执行上面的代码，它会产生下列结果：
```c
$./a.out testing1 testing2
Too many arguments supplied.
```
不传任何参数，编译并执行上面的代码，它会产生下列结果：
```c
$./a.out
One argument expected
```
应当指出的是，**argv[0] 存储程序的名称，argv[1] 是一个指向第一个命令行参数的指针**，*argv[n] 是最后一个参数。如果没有提供任何参数，argc 将为 1，否则，如果传递了一个参数，argc 将被设置为 2。
**多个命令行参数之间用空格分隔，但是如果参数本身带有空格，那么传递参数的时候应把参数放置在双引号 "" 或单引号 '' 内部**。让我们重新编写上面的实例，有一个空间，那么你可以通过这样的观点，把它们放在双引号或单引号""""。让我们重新编写上面的实例，向程序传递一个放置在双引号内部的命令行参数：
```c
#include <stdio.h>

int main( int argc, char *argv[] )  
{
   printf("Program name %s\n", argv[0]);
 
   if( argc == 2 )
   {
      printf("The argument supplied is %s\n", argv[1]);
   }
   else if( argc > 2 )
   {
      printf("Too many arguments supplied.\n");
   }
   else
   {
      printf("One argument expected.\n");
   }
}
```
使用一个用空格分隔的简单参数，参数括在双引号中，编译并执行上面的代码，它会产生下列结果：
```c
$./a.out "testing1 testing2"
Progranm name ./a.out
The argument supplied is testing1 testing2
```


