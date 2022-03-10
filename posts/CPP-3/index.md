# C++ Part-3


算法笔记知识点汇总-C++语法基础
<!--more-->

double类型,scanf需要使用"%lf"
字符串(char数组)scanf不需要&,例如: scanf("%s",str);
当需要申请较大空间的数组时,应将其定义在主函数的外面,因为在函数内部申请的局部变量来自于系统栈,函数外申请的全局变量来自于静态存储区
# 实用的输出格式
%md,%0md,%.mf
%md可以使不足m位的int型变量以m位进行右对齐输出,其中高位用空格补齐,如果本身超过m位,则保持原样
```c++
#include<stdio.h>

int main(){
    int a = 123, b=123456;
    printf("%5d\n, a");
    printf("%5d\n", b);
    return 0;
}
/*
输出结果:
  123
123456
*/
```
%0md即用0来代替空格进行补齐
%.mf代表小数点后保留几位

# 常用的math函数
fabs(double x): 对double类型变量取绝对值
floor(double x): 对double类型变量向下取整,返回类型为double
ceil(double x): 对double类型变量向上取整
pow(double a, double b): 用于返回 a<sup>b</sup>
sqrt(double x): 返回算数平方跟
log(double x): 返回以自然数为底的对数
在c语言中没有对任意底数求对数的函数,因此必须使用换底公式来进行求解
round(double x): 用于将double类型变量进行四舍五入,返回double类型

# memset
对数组中的每一个元素赋予相同的值 memset(a, 0, sizeof(a));

# gets()
对于scanf函数来说,其中的 %s 取字符串会通过**空格或换行符**来识别一个字符串的结束
因此,scanf("%s", &a); 无法全部取到 "hello world",遇到空格就会终止
gets()函数用于输入一整行字符串,因为其识别换行符作为输入的终止
# sscanf和sprintf
可以简单的将其看作,输入输出对象从screen换成了字符串数组

# 结构体的初始化: 构造函数
不需要写返回类型,并且函数名和结构体名字相同,一般来说,对于一个普通定义的结构体,其内部会默认生成一个构造函数
如果对构造函数进行自定义,可以写多个函数,以适应不同的初始化场合
```c++
struct studentinfo{
    int id;
    char gender;
    studentinfo(){};
    studentinfo(){
        gender = _gender;
    }
    studentinfo(){
        gender = _gender;
        id = _id;
    }
}
```

# EOF
当题目没有说明多少数据会输入时,可以利用scanf的返回值是否为EOF来判断输入是否结束(EOF = END OF FILE)
```C++
while(scanf("%d, &n") != EOF){
    /*code*/
}
```





