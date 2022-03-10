# C++ Part-2


第二部分: 面向对象-类
<!--more-->

# C++ 类成员函数
1. 成员函数可以定义在类定义内部
```c++
class Box
{
   public:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
      double getVolume(void)
      {
         return length * breadth * height;
      }
};
```
2. 单独使用范围解析运算符 :: 来定义
```c++
double Box::getVolume(void)
{
    return length * breadth * height;
}
```

## 实例
```c++
#include <iostream>
using namespace std;
 
class Box
{
   public:
      double length;         // 长度
      double breadth;        // 宽度
      double height;         // 高度
      // 成员函数声明
      double getVolume(void);
      void setLength( double len );
      void setBreadth( double bre );
      void setHeight( double hei );
};
 
// 成员函数定义
double Box::getVolume(void)
{
    return length * breadth * height;
}
void Box::setLength( double len )
{
    length = len;
}
void Box::setBreadth( double bre )
{
    breadth = bre;
}
void Box::setHeight( double hei )
{
    height = hei;
}
 
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   double volume = 0.0;     // 用于存储体积
   // box 1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
   // box 2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
   // box 1 的体积
   volume = Box1.getVolume();
   cout << "Box1 的体积：" << volume <<endl;
   // box 2 的体积
   volume = Box2.getVolume();
   cout << "Box2 的体积：" << volume <<endl;
   return 0;
}
```

# 类构造函数
```c++
#include <iostream>
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)    //在实例化时,构造函数自动执行
{
    cout << "Object is being created" << endl;
}
void Line::setLength( double len )
{
    length = len;
}
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
   return 0;
}
```

## 带参数的构造函数
```c++
#include <iostream>
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line(double len);  // 这是构造函数
   private:
      double length;
};
// 成员函数定义，包括构造函数
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
void Line::setLength( double len )
{
    length = len;
}
// 程序的主函数
int main( )
{
   Line line(10.0);
   line.setLength(6.0); 
   return 0;
}
```

## 使用初始化列表来初始化字段
```c++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}
```
上面的语法等同于如下语法：
```c++
Line::Line( double len)
{
    length = len;
    cout << "Object is being created, length = " << len << endl;
}
```
假设有一个类 C，具有多个字段 X、Y、Z 等需要进行初始化，同理地，您可以使用上面的语法，只需要在不同的字段使用逗号进行分隔，如下所示：
```c++
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```

# 类的析构函数
在每次删除所创建的对象时执行
```c++
#include <iostream>
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
   private:
      double length;
};
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
Line::~Line(void)
{
    cout << "Object is being deleted" << endl;
}

void Line::setLength( double len )
{
    length = len;
}
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
   return 0;
}
/*
当上面的代码被编译和执行时，它会产生下列结果：
Object is being created
Length of line : 6
Object is being deleted
*/
```

# C++ 拷贝构造函数
```c++
#include <iostream>
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
   private:
      int *ptr;
};
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line(10);
   display(line);
   return 0;
}
/*
调用构造函数
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
释放内存
*/
```
由于在执行display函数时会产生一个临时变量
随后调用拷贝构造函数把line的值给临时变量
等待函数执行完成后, 析构掉原始变量以及临时变量
