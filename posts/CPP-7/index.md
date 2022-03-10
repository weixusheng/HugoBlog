# C++ 申请空间


这几天每日六个多小时的数学课让我晕晕,码点代码来放松下心情吧
<!--more-->

# C malloc()
返回类型: **申请的同变量类型的指针**
```c++
typename* p = (typename*)malloc(sizeof(typename))
```
在使用malloc函数时,会向内存申请一块大小sizeof(node)的空间,并且返回指向这块空间的指针,但此时这个指针是一个未确定类型的指针 void* ,因此需要把他强制转换成为 node* 类型的指针,因此在malloc前面加上(node*),如果申请失败,会返回一个空指针NULL
free()对应于malloc()释放内存,参数为需要释放内存空间的指针变量

# C++ new()
new是c++中用来申请动态空间的运算符.其返回类型同样是申请的同变量类型的指针
```c++
typename* p = new typename
```
delete()对应于new()参数也是对应空间的指针

