# C++ fiil()与memset()


fill() 与 memset() 的区别<br>
isalnum() 函数
<!--more-->

# fill与memset的区别
## fill
作用:按照单元赋值，将一个区间的元素都赋予val值。函数参数：fill(vec.begin(), vec.end(), val); val为将要替换的值
```c++
 #include <algorithm>
fill(vec.begin(), vec.end(), val); //原来容器中每个元素被重置为val
```
## memset
作用：按照字节填充某字符
```c++
#include <cstring>
const int INF = 0x3f3f3f3f;
memset(a,INF,sizeof(a));
```

## 赋值的区别
### memset函数
因为memset函数按照字节填充，所以一般memset只能用来填充char型数组，（因为只有char型占一个字节）。
如果**填充int型数组，只能填充0、-1 和 inf（正负）**
因为00000000 = 0，-1同理，如果我们把每一位都填充“1”，会导致变成填充入“11111111”。如果我们将inf设为0x3f3f3f3f，0x3f3f3f3f的每个字节都是0x3f！
**全部置为无穷大，只需要memset(a,0x3f,sizeof(a))**
**全部置为无穷小可以将-INF设为0x8f**

### fill函数
fill函数可以赋值任何值

## 运行效率
memset比fill处理速度快一些，所以在能满足需要时，推荐用memset

# isalnum()函数
void isalnum(int c) **检查所传的字符是否是字母或数字**
```c++
int isalnum(int c); // c 为被检查的字符
/*
返回值:
如果 c 是一个数字或一个字母，则该函数返回非零值，否则返回 0。
*/
```
