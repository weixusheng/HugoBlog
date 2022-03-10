# yield的用法



Python Generator
<!--more-->


## 1.生成器

- 在 Python 中，一边循环一边计算的机制，称为生成器（Generator）；
- 生成器是一个返回迭代器的函数，只能用于迭代操作；

## 2.什么是生成器函数

- 生成器是Python中的一个对象，对这个对象进行操作，可以依次生产出按生成器内部运算产生的数据；
- 生成器函数指的是函数体中包含yield关键字的函数（yield就是专门给生成器用的return）；
- 生成器可以通过生成器表达式和生成器函数获取到；

## 生成器语法

### 生成器表达式

通列表解析语法，只不过把列表解析的[]换成()
生成器表达式能做的事情列表解析基本都能处理，只不过在需要处理的序列比较大时，列表解析比较费内存。

```python
>>> gen = (x**2 for x in range(5))
>>> gen
<generator object <genexpr> at 0x0000000002FB7B40>
>>> for g in gen:
...   print(g, end='-')
...
0-1-4-9-16-
>>> for x in [0,1,2,3,4,5]:
...   print(x, end='-')
...
0-1-2-3-4-5-
```

### 生成器函数

在函数中如果出现了yield关键字，那么该函数就不再是普通函数，而是生成器函数。
但是生成器函数可以生产一个无线的序列，这样列表根本没有办法进行处理。
yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator

```python
def odd():
    n=1
    while True:
        yield n
        n+=2
odd_num = odd()
count = 0
for o in odd_num:
    if count >=5: break
    print(o)
    count +=1
```

### `send()`

生成器函数最大的特点是可以接受外部传入的一个变量，并根据变量内容计算结果后返回。

```python
def gen():
    value=0
    while True:
        receive = yield value
        if receive == 'e':
            break
        value = 'got: %s' % receive

g=gen()
print(g.send(None))     
print(g.send('aaa'))
print(g.send(3))
print(g.send('e'))
```

**输出**

```python
"""
1. 通过g.send(None)或者next(g)可以启动生成器函数，并执行到第一个yield语句结束的位置。
   此时，执行完了yield语句，但是没有给receive赋值。
   yield value会输出初始值0
   注意：在启动生成器函数时只能send(None),如果试图输入其它的值都会得到错误提示信息。
2. 通过g.send('aaa')，会传入aaa，并赋值给receive，然后计算出value的值，并回到while头部，执行yield value语句有停止。
   此时yield value会输出"got: aaa"，然后挂起。
3. 通过g.send(3)，会重复第2步，最后输出结果为"got: 3"
4. 当我们g.send('e')时，程序会执行break然后推出循环，最后整个函数执行完毕，所以会得到StopIteration异常。
   最后的执行结果如下：
"""
0
got: aaa
got: 3
Traceback (most recent call last):
File "h.py", line 14, in <module>
  print(g.send('e'))
StopIteration
```

### `throw()`

用来向生成器函数送入一个异常，可以结束系统定义的异常，或者自定义的异常。
throw()后直接跑出异常并结束程序，或者消耗掉一个yield，或者在没有下一个yield的时候直接进行到程序的结尾。

```python
def gen():
    while True: 
        try:
            yield 'normal value'
            yield 'normal value 2'
            print('here')
        except ValueError:
            print('we got ValueError here')
        except TypeError:
            break

g=gen()
print(next(g))
print(g.throw(ValueError))
print(next(g))
print(g.throw(TypeError))
```

**输出**

```python
"""
1. print(next(g))：会输出normal value，并停留在yield 'normal value 2'之前。
2. 由于执行了g.throw(ValueError)，所以会跳过所有后续的try语句，也就是说yield 'normal value 2'不会被执行，然后进入到except    语句，打印出we got ValueError here。
3. 然后再次进入到while语句部分，消耗一个yield，所以会输出normal value。
4. print(next(g))，会执行yield 'normal value 2'语句，并停留在执行完该语句后的位置。
5. g.throw(TypeError)：会跳出try语句，从而print('here')不会被执行，然后执行break语句，跳出while循环，然后到达程序结尾，所    以跑出StopIteration异常。
"""
normal value
we got ValueError here
normal value
normal value 2
Traceback (most recent call last):
  File "h.py", line 15, in <module>
    print(g.throw(TypeError))
StopIteration
```

### `yield from`

yield产生的函数就是一个迭代器，所以我们通常会把它放在循环语句中进行输出结果。
有时候我们需要把这个yield产生的迭代器放在另一个生成器函数中，也就是生成器嵌套。
比如下面的例子：

```python
def inner():
    for i in range(10):
        yield i
def outer():
    g_inner=inner()    #这是一个生成器
    while True:
        res = g_inner.send(None)
        yield res

g_outer=outer()
while True:
    try:
        print(g_outer.send(None))
    except StopIteration:
        break
```

此时，我们可以采用yield from语句来减少我么你的工作量。

```python
def outer2():
    yield from inner()
```

## 3.生成器函数的定义

```python
def add():    
	for i in range(10):       
        yield i
g = add()
print(g)  # <generator object add at 0x10f6110f8>
print(next(g))  # 0
print(next(g))  # 1
```

- 我们可以通过yield关键字来定义一个生成器函数，这个生成器函数返回值就是一个生成器对象；

## 4.生成器函数的调用

```python
def gen():    
    print('111111')    
    yield '111111'    
    print('222222')    
    yield '222222'    
    print('333333')    
    yield '333333'
g = gen()
print(g)  # <generator object gen at 0x0026BBF0>
next(g)   # 111111
next(g)   # 222222
next(g)   # 333333
next(g, 'over')
```

- 生成器函数可以使用next()迭代，且每次next()只会返回一次yield的值，然后暂停，下次一次next()时会在当前位置继续，如果没有元素可以迭代了，还 执在行next()则需要给定一个默认值，不给默认值会报错；
- 如果在生成器函数中使用return，则会终止迭代，且不能得到返回值；

```python
def gen():    
    print('111111')    
    yield '111111'    
    print('222222')    
    return '222222'    
	print('333333')    
    yield '333333'
g = gen()
print(g)  # <generator object gen at 0x0026BBF0>
next(g)   # 111111
next(g)   # 222222, 抛出异常
```

## 5.生成器函数的使用场景

```python
# 死循环
def way():    
    i = 0    
    while True:        
        i += 1        
        yield i
c = way()
print(next(c)) # 1
print(next(c)) # 2
print(next(c)) # 3
print(next(c)) # 4
print(next(c)) # 5
```

- 在生成器中使用死循环，不会一直执行，仍旧是执行多少次next()，返回多少个值；

## 6.生成器函数中的语法糖

```python
# 普通生成器函数way1
def way1():    
    for i in range(5):        
        yield i
# 带语法糖的生成器函数way2
def way2():    
    yield from range(5)
#循环输出way1
for i in way1():    
    print(i)  #0 1 2 3 4
#循环输出way2
for j in way2():    
    print(j)  #0 1 2 3 4
```

## 生成器的实例

```python
def eat(name):
    print('%s ready to eat' % name)
    food_list = []
    while True:
        food = yield food_list  # food='骨头'
        food_list.append(food)  # food_list=['泔水','骨头']
        print('%s start to eat %s' % (name, food))


dog1 = eat('alex')
# 1、必须初始化一次，让函数停在yield的位置
res0 = dog1.__next__()
print(res0)
# 接下来的事，就是喂狗,send有两方面的功能
# 1、给yield传值
# 2、同__next__的功能
res1 = dog1.send('泔水')
print(res1)
res2 = dog1.send('骨头')
print(res2)
res3 = dog1.send('shit')
print(res3)

"""控制台打印

alex ready to eat
[]
alex start to eat 泔水
['泔水']
alex start to eat 骨头
['泔水', '骨头']
alex start to eat shit
['泔水', '骨头', 'shit']

"""
```


