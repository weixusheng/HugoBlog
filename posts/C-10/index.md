# C-数组形参的长度



刷leetcode的时候,由于报错显示的很简单,想调试程序的话有些困难,所以就想在c环境下断点调试代码,今天做一道关于树的题的时候,遇到了这个问题,就是数组形参的长度永远都是8字节,就很离谱...后来才发现了缘故
原来在字符型的数组中我们可以使用 strlen() 来获取当前数组的长度，对于其他类型的数组，这个方法就不适用了.
<!--more-->

# C-数组形参获得长度
```c
# include<stdio.h>

int main(int argc, char * argv[])
{
  int a[] = {2, 6, 3, 5, 9};
  p = a;
  printf("The length is: %d\n",length(a));
  printf("The length is: %d\n",sizeof(a)/sizeof(int));
  printf("The length of pointer is: %d\n", sizeof(p));
  return 0;
}
int length(int a)
{
  int length;
  length =  sizeof(a)/ sizeof(int);
  return length;
}
```
执行结果：
```
The length is: 2
The length is: 5
The length of pointer is: 8
```
a[]是长度计算的形式参数，在 main()函数中调用时，a是一个指向数组第一个元素的指针。在执行main() 函数时，不知道a所表示的地址有多大的数据存储空间，只是告诉函数：一个数据存储空间首地址。

sizeof(a)的结果是指针变量a占内存的大小，一般在64位机上是8个字节。
a[0]是 int 类型，sizeof a[0]是4个字节，结果是2。

所以,形参中只是指针,无法通过计算得到数组的长度,只有提前将长度算好传入函数



