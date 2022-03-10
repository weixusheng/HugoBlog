# C语言的类型定义


typedef的用法,typedef与const
<!--more-->

## typedef的用法
typedef关键字可以用于给数据类型定义一个别名，比如说你本名叫关谷神奇，我嫌弃这个名字太长了，所以给你取一个别名，叫关谷，以后我叫关谷的时候你就知道在叫你了。
当你定义了一个结构体时，每次创建一个结构体都要使用struct+结构体名的方式，而用了typedef之后，只要一个结构体别名就可以创建了。
```c
struct Color{ //本名
    unsigned char RED;
    unsigned char GREEN;
    unsigned char BLUE;
};
struct Clolor color1; //此时声明需要struct

typedef struct { //本名可以忽略
    unsigned char RED;
    unsigned char GREEN;
    unsigned char BLUE;
}Color; //结构体别名
Color color1； //此时声明不需要struct
```

## typedef与const
在和const一起使用的时候，本想定义一个指向的字符为常量的变量指针，但因为typedef的特殊性，不是简单的替换，所以最终的定义的是指向的字符为变量的常量指针。
```c
typedef char* pcha; //定义字符指针的别名
int str(const pchar, const pchar)
/*想定义 指针为变量， 指向的字符为常量的参数
但实际上 指针为常量， 指向的字符为变量*/
```
解决的方法，在typedef中加入const
```c
typedef const char* pchar;
int str(pchar, pchar);
```

