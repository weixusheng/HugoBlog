# C常用库函数整理


史上最全
<!--more-->

# 标准库<stdlib.h>
## calloc
void *calloc(unsigned n,unsigned size)
 - 功能:分配n个数据项的内存空间，每个数据项的大小为size个字节
 - 返回:分配内存单元的起始地址；如不成功，返回0

## free
void *free(void *p)
 - 功能:释放p所指的内存区
 - 返回:无

## malloc
void *malloc(unsigned size)
 - 功能:分配size个字节的存储空间
 - 返回:分配内存空间的地址；如不成功，返回0

## realloc
void *realloc(void *p,unsigned size)
 - 功能:把p所指内存区的大小改为size个字节
 - 返回:新分配内存空间的地址；如不成功，返回0

## rand
int rand(void)
 - 功能:产生0～32767的随机整数
 - 返回:返回一个随机整数

## exit
void exit(int state)
 - 功能:程序终止执行，返回调用过程，state为0正常终止，非0非正常终止
 - 返回:无

# 字符串库<string.h>
## 字符串操作
### strcat
char *strcat(char *s1,char *s2)
 - 功能:把字符串s2接到s1后面
 - 返回:s1所指地址

### strchr
char *strchr(char *s,int ch)
 - 功能:在s所指字符串中，找出第一次出现字符ch的位置
 - 返回:返回找到的字符的地址，找不到返回NULL	

### strcmp
int strcmp(char *s1,char *s2)
 - 功能:对s1和s2所指字符串进行比较
 - 返回:s1小于s2,返回负数；s1相等s2,返回0；s1大于s2,返回正数

### strcpy
char *strcpy(char *s1,char *s2)
 - 功能:把s2指向的串复制到s1指向的空间
 - 返回:s1 所指地址

### strlen
unsigned strlen(char *s)
 - 功能:求字符串s的长度
 - 返回:返回串中字符（不计最后的'\0'）个数

### strstr
char *strstr(char *s1,char *s2)
 - 功能:在s1所指字符串中，找出字符串s2第一次出现的位置
 - 返回:返回找到的字符串的地址，找不到返回NULL

## 存储区操作
参数s的类型是(void *)，cs和ct的类型是(const void *)，n的类型是size_t，c的类型是int（转换为unsigned char）。
### memset
void *memset(s,c,n)
将s的前n个字符设置为c，返回s

### memcpy
void *memcpy(s,ct,n)
从ct处复制n个字符到s处，返回s

### memcmp
int memcmp(cs,ct,n)
比较由cs和ct开始的n个字符，返回值定义同strcmp

### memmove
void *memmove(s,ct,n)
从ct处复制n个字符到s处，返回s，这里的两个段允许重叠

### memchr
void *memchr(cs,c,n)
在n个字符的范围内查寻c在cs中的第一次出现，如果找到，返回该位置的指针值，否则返回NULL

## memcmp与strcmp的区别
### memcmp原型
int memcmp(const void *buf1, const void *buf2, unsigned int count);
 - 功能:比较内存区域buf1和buf2的前count个字节。
 - 返回值
    1. 当buf1 < buf2时，返回值<0
    2. 当buf1==buf2时，返回值=0
    3. 当buf1 > buf2时，返回值>0
 - 说明
该函数是按字节比较的。
 - 举例:
s1,s2为字符串时候memcmp(s1,s2,1)就是比较s1和s2的第一个字节的ascII码值；
memcmp(s1,s2,n)就是比较s1和s2的前n个字节的ascII码值；
如:char *s1="abc";
char *s2="acd";
int r=memcmp(s1,s2,3);
就是比较s1和s2的前3个字节，第一个字节相等，第二个字节比较中大小已经确定，不必继续比较第三字节了。所以r=-1

### 区别
 - 对于memcmp()，如果两个字符串相同而且count大于字符串长度的话，memcmp不会在\0处停下来，会继续比较\0后面的内存单元，直到_res不为零或者达到count次数。      
 - 对于strncmp()，由于((__res = *cs - *ct++) != 0 || !*cs++)的存在，比较必定会在最短的字符串的末尾停下来，即使count还未为零。
 - 具体的例子：      
char a1[]="ABCD";   
char a2[]="ABCD";       
对于memcmp(a1,a2,10)，memcmp在两个字符串的\0之后继续比较   
对于strncmp(a1,a2,10），strncmp在两个字符串的末尾停下，不再继续比较。       
所以，如果想**使用memcmp比较字符串，要保证count不能超过最短字符串的长度**，否则结果有可能是错误的。

## strcpy和memcpy的区别
1. 复制的内容不同。strcpy只能复制字符串，而memcpy可以复制任意内容，例如字符数组、整型、结构体、类等。
2. 复制的方法不同。strcpy不需要指定长度，它遇到被复制字符的串结束符"\0"才结束，所以容易溢出。memcpy则是根据其第3个参数决定复制的长度。
3. 用途不同。通常在复制字符串时用strcpy，而需要复制其他类型数据时则一般用memcpy

# 输入输出函数<stdio.h>
## 文件操作
### fopen
FILE *fopen(char *filename,char *mode)
 - 功能:以mode指定的方式打开名为filename的文件
 - 返回:成功，返回文件指针（文件信息区的起始地址），否则返回NULL

### fclose
int fclose(FILE *fp)
 - 功能:关闭fp所指的文件，释放文件缓冲区
 - 返回:出错返回非0，否则返回0

### feof
int feof (FILE *fp)
 - 功能:检查文件是否结束
 - 返回:遇文件结束返回非0，否则返回0

### clearer
void clearer(FILE *fp)
 - 功能:清除与文件指针fp有关的所有出错信息
 - 返回:无

### rename
int rename(char *oldname,char *newname)
 - 功能:把oldname所指文件名改为newname所指文件名
 - 返回:成功返回0，出错返回-1

## GET
### getc
int getc (FILE *fp)
 - 功能:从fp所指文件中读取一个字符
 - 返回:返回所读字符，若出错或文件结束返回EOF

### fgetc
int fgetc (FILE *fp)
 - 功能:从fp所指的文件中取得下一个字符
 - 返回:出错返回EOF，否则返回所读字符

### fgets
char *fgets(char *buff,int n, FILE *fp)
 - 功能:从fp所指的文件中读取一个长度为n-1的字符串，将其存入buff所指存储区
 - 返回:返回buf所指地址，若遇文件结束或出错返回NULL

### getchar
int getchar(void)
 - 功能:从标准输入设备读取下一个字符
 - 返回:返回所读字符，若出错或文件结束返回-1

### gets
char *gets(char *s)
 - 功能:从标准设备读取一行字符串放入s所指存储区，用’\0’替换读入的换行符
 - 返回:返回s,出错返回NULL


## PUT
### putc
int putc (int ch, FILE *fp)
 - 功能:同fputc
 - 返回:同fputc

### fputc
int fputc(char ch, FILE *fp)
 - 功能:把ch中字符输出到fp指定的文件中
 - 返回:成功返回该字符，否则返回EOF

### fputs
int fputs(char *str, FILE *fp)
 - 功能:把str所指字符串输出到fp所指文件
 - 返回:成功返回非负整数，否则返回-1（EOF）

### putchar
int putchar(char ch)
 - 功能:把ch输出到标准输出设备
 - 返回:返回输出的字符，若出错则返回EOF

### puts
int puts(char *str)
 - 功能:把str所指字符串输出到标准设备，将’\0’转成回车换行符
 - 返回:返回换行符，若出错，返回EOF

## 输入
### scanf
int scanf(char *format,args,…)
 - 功能:从标准输入设备按format指定的格式把输入数据存入到args,…所指的内存中
 - 返回:已输入的数据的个数

### fscanf
int fscanf (FILE *fp, char *format,args,…)
 - 功能:从fp所指的文件中按format指定的格式把输入数据存入到args,…所指的内存中
 - 返回:已输入的数据个数，遇文件结束或出错返回0

## 输出
### printf
int printf(char *format,args,…)
 - 功能:把args,…的值以format指定的格式输出到标准输出设备
 - 返回:输出字符的个数

### fprintf
int fprintf(FILE *fp, char *format, args,…)
 - 功能:把args,…的值以format指定的格式输出到fp指定的文件中
 - 返回:实际输出的字符数

## 文件读写
### fread
int fread(char *pt,unsigned size,unsigned n, FILE *fp)
 - 功能:从fp所指文件中读取长度size为n个数据项存到pt所指文件
 - 返回:读取的数据项个数

### fwrite
int fwrite(char *pt,unsigned size,unsigned n, FILE *fp)
 - 功能:把pt所指向的n*size个字节输入到fp所指文件
 - 返回:输出的数据项个数

## 文件指针操作
### fseek
int fseek (FILE *fp,long offer,int base)
 - 功能:移动fp所指文件的位置指针
 - 返回:成功返回当前位置，否则返回非0

### ftell
long ftell (FILE *fp)
 - 功能:求出fp所指文件当前的读写位置
 - 返回:读写位置，出错返回 -1L

### rewind
void rewind(FILE *fp)
 - 功能:将文件位置指针置于文件开头
 - 返回:无





