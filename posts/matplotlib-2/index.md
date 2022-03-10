# Matplotlib 进阶


Python数据可视化
<!--more-->

# Subplot多图合一
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/2/0_.png">
```python
import matplotlib.pyplot as plt

plt.figure()
plt.subplot(2, 1, 1)
plt.plot([0, 1], [0, 1])
plt.subplot(2, 2, 3)
plt.plot([0, 1], [0, 2])
plt.subplot(2, 2, 4)
plt.plot([0, 1], [0, 3])
plt.show()  # 展示
```
使用plt.subplot来创建小图
plt.subplot(2,2,1)表示将整个图像窗口分为2行1列, 当前位置为1.
使用plt.plot([0,1],[0,1])在第1个位置创建一个小图.
plt.subplot(2,2,3)表示将整个图像窗口分为2行2列, 当前位置为3. 
因为在第一行中占了两个位置
使用plt.plot([0,1],[0,2])在第3个位置创建一个小图.
plt.subplot(2,2,4)表示将整个图像窗口分为2行2列,当前位置为4. 
plt.subplot(2,2,4)可以简写成plt.subplot(224), matplotlib同样可以识别. 
使用plt.plot([0,1],[0,3])在第3个位置创建一个小图

## subplot2grid
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/2/1_.png">
将画布分为表格形式,分格排列
```python
import matplotlib.pyplot as plt

plt.figure()
ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
ax1.plot([1, 2], [1, 2])    # 画小图
ax1.set_title('ax1_title')  # 设置小图的标题
ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
ax1.plot([1, 2], [1, 2])    # 画小图
ax1.set_title('ax1_title')  # 设置小图的标题
ax2 = plt.subplot2grid((3, 3), (1, 0), colspan=2)
ax3 = plt.subplot2grid((3, 3), (1, 2), rowspan=2)
ax4 = plt.subplot2grid((3, 3), (2, 0))
ax5 = plt.subplot2grid((3, 3), (2, 1))
ax4.scatter([1, 2], [2, 2])
ax4.set_xlabel('ax4_x')
ax4.set_ylabel('ax4_y')
```
### 第一个小图
使用plt.subplot2grid来创建第1个小图
```python
ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
ax1.plot([1, 2], [1, 2])    # 画小图
ax1.set_title('ax1_title')  # 设置小图的标题
```

|         参数         |             作用             |
| :------------------: | :--------------------------: |
|        (3,3)         | 表示将整个图像窗口分成3行3列 |
|        (0,0)         |   表示从第0行第0列开始作图   |
|      colspan=3       |       表示列的跨度为3        |
|      rowspan=1       |       表示行的跨度为1        |
| colspan和rowspan缺省 |         默认跨度为1          |

### 第二个小图
使用plt.subplot2grid来创建第2个小图
```python
ax2 = plt.subplot2grid((3, 3), (1, 0), colspan=2)
```

|   参数    |             作用             |
| :-------: | :--------------------------: |
|   (3,3)   | 表示将整个图像窗口分成3行3列 |
|   (1,0)   |   表示从第1行第0列开始作图   |
| colspan=2 |       表示列的跨度为2        |
|   (1,2)   |   表示从第1行第2列开始作图   |
| rowspan=2 |       表示行的跨度为2        |

### 第三个小图
```python
ax3 = plt.subplot2grid((3, 3), (1, 2), rowspan=2)
```

### 第四和第五小图
```python
ax4 = plt.subplot2grid((3, 3), (2, 0))
ax4.scatter([1, 2], [2, 2])
ax4.set_xlabel('ax4_x')
ax4.set_ylabel('ax4_y')
ax5 = plt.subplot2grid((3, 3), (2, 1))
```

## gridspec
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/2/2_.png">
```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec

plt.figure()
gs = gridspec.GridSpec(3, 3)
ax6 = plt.subplot(gs[0, :])
ax7 = plt.subplot(gs[1, :2])
ax8 = plt.subplot(gs[1:, 2])
ax9 = plt.subplot(gs[-1, 0])
ax10 = plt.subplot(gs[-1, -2])
```
使用plt.figure()创建一个图像窗口
**使用gridspec.GridSpec将整个图像窗口分成3行3列**

**使用plt.subplot来作图**
 - gs[0, :]表示这个图占第0行和所有列
 - gs[1, :2]表示这个图占第1行和第2列前的所有列
 - gs[1:, 2]表示这个图占第1行后的所有行和第2列
 - gs[-1, 0]表示这个图占倒数第1行和第0列
 - gs[-1, -2]表示这个图占倒数第1行和倒数第2列

# 绘制途中图
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/2/3_.png">
```python
import matplotlib.pyplot as plt

fig = plt.figure()
x = [1, 2, 3, 4, 5, 6, 7]
y = [1, 3, 4, 2, 5, 8, 6]
# 确定大图左下角的位置以及宽高
left, bottom, width, height = 0.1, 0.1, 0.8, 0.8
# 添加大图的坐标系
ax1 = fig.add_axes([left, bottom, width, height])
ax1.plot(x, y, 'r')
ax1.set_xlabel('x')
ax1.set_ylabel('y')
ax1.set_title('title')
# 绘制第一个小图
left, bottom, width, height = 0.2, 0.6, 0.25, 0.25
ax2 = fig.add_axes([left, bottom, width, height])
ax2.plot(y, x, 'b')
ax2.set_xlabel('x')
ax2.set_ylabel('y')
ax2.set_title('title inside 1')
# 绘制第二个小图
plt.axes([0.6, 0.2, 0.25, 0.25])
plt.plot(y[::-1], x, 'g') # 注意对y进行了逆序处理
plt.xlabel('x')
plt.ylabel('y')
plt.title('title inside 2')
```

## left,bottom,width,height
```python
left, bottom, width, height = 0.1, 0.1, 0.8, 0.8
```
这四个值都是占整个figure坐标系的百分比
**假设figure的大小是10x10，那么大图就被包含在由(1, 1)开始，宽8，高8的坐标系内**

# 建立次要坐标轴
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/2/4_.png">
```python
fig = plt.figure()
x = np.arange(0, 10, 0.1)
y1 = 0.05 * x**2
y2 = -1 * y1
fig, ax1 = plt.subplots()
ax2 = ax1.twinx() # 生成镜面效果后的坐标轴
ax1.plot(x, y1, 'g-')   # green, solid line
ax1.set_xlabel('X data')
ax1.set_ylabel('Y1 data', color='g')
ax2.plot(x, y2, 'b-') # blue
ax2.set_ylabel('Y2 data', color='b')

```
subplots返回的值的类型为元组，其中包含两个元素：第一个为一个画布，第二个是子图 

# code
```python
import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
import numpy as np

plt.figure()
plt.subplot(2, 1, 1)
plt.plot([0, 1], [0, 1])
plt.subplot(2, 2, 3)
plt.plot([0, 1], [0, 2])
plt.subplot(2, 2, 4)
plt.plot([0, 1], [0, 3])

plt.figure()
ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
ax1.plot([1, 2], [1, 2])    # 画小图
ax1.set_title('ax1_title')  # 设置小图的标题
ax1 = plt.subplot2grid((3, 3), (0, 0), colspan=3)
ax1.plot([1, 2], [1, 2])    # 画小图
ax1.set_title('ax1_title')  # 设置小图的标题
ax2 = plt.subplot2grid((3, 3), (1, 0), colspan=2)
ax3 = plt.subplot2grid((3, 3), (1, 2), rowspan=2)
ax4 = plt.subplot2grid((3, 3), (2, 0))
ax5 = plt.subplot2grid((3, 3), (2, 1))
ax4.scatter([1, 2], [2, 2])
ax4.set_xlabel('ax4_x')
ax4.set_ylabel('ax4_y')

plt.figure()
gs = gridspec.GridSpec(3, 3)
ax6 = plt.subplot(gs[0, :])
ax7 = plt.subplot(gs[1, :2])
ax8 = plt.subplot(gs[1:, 2])
ax9 = plt.subplot(gs[-1, 0])
ax10 = plt.subplot(gs[-1, -2])

fig = plt.figure()
x = [1, 2, 3, 4, 5, 6, 7]
y = [1, 3, 4, 2, 5, 8, 6]
# 确定大图左下角的位置以及宽高
left, bottom, width, height = 0.1, 0.1, 0.8, 0.8
# 添加大图的坐标系
ax1 = fig.add_axes([left, bottom, width, height])
ax1.plot(x, y, 'r')
ax1.set_xlabel('x')
ax1.set_ylabel('y')
ax1.set_title('title')
# 绘制第一个小图
left, bottom, width, height = 0.2, 0.6, 0.25, 0.25
ax2 = fig.add_axes([left, bottom, width, height])
ax2.plot(y, x, 'b')
ax2.set_xlabel('x')
ax2.set_ylabel('y')
ax2.set_title('title inside 1')
# 绘制第二个小图
plt.axes([0.6, 0.2, 0.25, 0.25])
plt.plot(y[::-1], x, 'g')  # 注意对y进行了逆序处理
plt.xlabel('x')
plt.ylabel('y')
plt.title('title inside 2')

x = np.arange(0, 10, 0.1)
y1 = 0.05 * x**2
y2 = -1 * y1
fig, ax1 = plt.subplots()
ax2 = ax1.twinx()  # 生成镜面效果后的坐标轴
ax1.plot(x, y1, 'g-')   # green, solid line
ax1.set_xlabel('X data')
ax1.set_ylabel('Y1 data', color='g')
ax2.plot(x, y2, 'b-')  # blue
ax2.set_ylabel('Y2 data', color='b')

plt.show()  # 展示
```
