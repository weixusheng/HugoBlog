# Python(NCRE)


二级Python
<!--more-->

## 输出格式

### 格式要求:宽度为20,超过20则显示本来的字符串

```python
n = eval(input("请输入正整数:"))
print("{:->20,}".format(n))
```

### ":"表示填充,其后面的字符为所要填充的字符

```python
impor random

random.seed(123) #随机数种子
for i in range(4):
    print(random.randint(1,1000),end=",")	#random.randint()生成随机数
```

## turtle库的使用

```python
import turtle
turtle.right(-30) #right顺时针旋转角度
turtle.fd(200) #绘画线
turtle.right(60)
turtle.fd(200)
turtle.right(120)
turtle.fd(200)
turtle.right(60)
turtle.fd(200)
turtle.right(120)
```

left()、right() 使用时，角度以当前所在位置方向为参照，是**相对角度**

seth()使用时，角度以坐标系原点为参照，是**绝对角度**

## 字符串处理题

```python
txt=open("命运.txt","r").read()	#读取文件
for ch in"，。？：":	#去除特殊符号
    txt=txt.replace(ch,"")
d = {}
for ch in txt:
    d[ch]=d.get(ch,0)+1	#字符串计数
ls=list(d.items())	#转换为元组
ls.sort(key=lambda x:x[1], reverse=True)	#
a,b=ls[0]
print("{}:{}".format(a,b))
```

## Python字典转换为元组

```python
dict.items()
# Python 字典(Dictionary) items() 函数以列表返回可遍历的(键, 值) 元组数组。
```

## Python元组排序

1. 如果是对于key排序,直接可以

   ```python
   sorted(d.keys(), reverse=True) # reverse表示逆序排序
   ```

2. 对于键值value排序,需要用到d.items()实际上是将d转换为可迭代对象

   例如: 迭代对象的元素为(‘lilee’,25),(‘wangyan’,21),(‘liqun’,32),(‘lidaming’,19);

   items()方法将字典的元素转化为了元组，**而这里key参数对应的lambda表达式**的意思则是选取元组中的第二个元素作为比较参数（如果写作**key=lambda item:item[0]**的话则是**选取第一个元素作为比较对象**，也就是key值作为比较对象。lambda x:y中x表示输出参数，y表示lambda函数的返回值）

   所以采用这种方法可以对字典的value进行排序。注意排序后的返回值是一个list，而原字典中的名值对被转换为了list中的元组。

   ```python
   sorted(d.items(), key=lambda item:item[0], reverse=True)
   ```




