# C++ stoi()函数


C++ stoi()函数用法
<!--more-->

# 头文件:
```c++
#include <string>
```

# 用法：
stoi（字符串，起始位置，n进制）
**将 n 进制的字符串转化为 10 进制**
```c++
//将字符串 str 从 0 位置开始到末尾的 2 进制转换为十进制
stoi(str, 0, 2); 
```

# 实例:

```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string str = "1010";
    int a = stoi(str, 0, 2);
    cout << a << endl;
    return 0;  // 输出 10
}
```

