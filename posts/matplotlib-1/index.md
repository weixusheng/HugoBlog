# Matplotlib 绘制基本图表


Python数据可视化
<!--more-->

这两天杂事有点多...学的东西也比较杂,所以最后笔记几天都没更新
这两天慢慢把笔记补全,今天来补一下matplotlib笔记
一开始是在看tensorflow,后来看上了pytorch,再后来发现数据可视化表示很重要
因此想了想还是先学一下最基本的matplotlib吧
其实没有想象中的那么难,他本身已经封装的很傻瓜了,因此只要有数据,几步就能把数据图画出来
笔记的内容不是很系统,都是东拼西凑学来的

# 基本概念
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/1/3_.png">
```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

x = np.linspace(-1, 3, 50)
y1 = 2*x + 1
y2 = x**2
plt.figure()
plt.plot(x, y1, label="line-1")
plt.plot(x, y2, color='red', linewidth=1.0, linestyle=':', label="line-2")
# plt.xlim((-1, 3))  # 设定X轴的坐标范围
# plt.ylim((-2, 7))  # 设定Y轴的坐标范围
# 创建坐标轴刻度数据
x_ticks = np.linspace(-1, 3, 5)
y_ticks = np.linspace(-2, 7, 10)
plt.xticks(x_ticks)
plt.yticks(y_ticks)
'''
# 设定Y坐标轴为特定符号
plt.yticks([-1, 0, 1, 2],
           [r'$\alpha$', r'$\beta$', r'$c$', r'$d$'])
'''
# 获取当前的坐标系
ax = plt.gca()
# 设置刻度线显示位置
ax.xaxis.set_ticks_position('top')
# 设定坐标轴的文字
plt.xlabel('I am x')  # X轴的名称
plt.ylabel('I am y')  # Y轴的名称
# 设定坐标轴为指定颜色
ax.spines['right'].set_color('red')
ax.spines['top'].set_color('red')
# 设定坐标轴位置
ax.spines['bottom'].set_position(('data', -2))
# 垂直坐标轴指示点-虚线
x0 = 1
y0 = 2*x0 + 1
plt.plot([x0, x0, ], [-2, y0, ], 'k--', linewidth=2.5)
# 垂直坐标轴指示点-绘制点
plt.scatter([x0, ], [y0, ], s=50, color='b')
# 曲线注释
plt.annotate(r'$2x+1=%s$' % y0, xy=(x0, y0), xycoords='data', xytext=(+30, -30),
             textcoords='offset points', fontsize=16,
             arrowprops=dict(arrowstyle='->', connectionstyle="arc3,rad=.2"))
# 标注文字
plt.text(-1, 5, r'$This\ is\ the\ some\ text. \mu\ \sigma_i\ \alpha_t$',
         fontdict={'size': 16, 'color': 'r'})
# 显示指示框
plt.legend(loc="best")
plt.show()

```

## numpy.linspace 
numpy.linspace(1,20,20)"表示-> 在[1,20]范围内选取20个点
```python
test_trick = np.linspace(1, 20, 20)
print(test_trick)
# [ 1.  2.  3.  4.  5.  6.  7.  8.  9. 10. 11. 12. 13. 14. 15. 16. 17. 18.19. 20.]
```

## plt.figure 
表示创建一个画布,图表中的元素都是向画布中添加

|    参数     |                              用处                              |
| :---------: | :------------------------------------------------------------: |
|     num     |       表示图形的编号或名称，数字代表编号，字符串表示名称       |
|   figsize   |           用于设置画布的尺寸，宽度、高度以英寸为单位           |
|     dpi     |                      用于设置图形的分辨率                      |
|  facecolor  |                     用于设置画板的背景颜色                     |
|  edgecolor  |                       用于显示边框的颜色                       |
|   frameon   |                        表示是否显示边框                        |
| FigureClass | 派生自matplotlib.figure.Figure的类，可以选择用自定义的图形对象 |
|    clear    |            若设为True且改图形已经存在，则它会被清除            |

[参考文献](https://blog.csdn.net/missyougoon/article/details/90399938)

## plt.plot
绘制曲线

|   参数    |    用处    |
| :-------: | :--------: |
|   alpha   | 曲线透明度 |
|   color   |  曲线颜色  |
|   label   | 曲线的命名 |
| linestyle | 曲线的样式 |
| linewidth |  曲线粗细  |

### linestyle:
| 符号  |  样式  |
| :---: | :----: |
|   :   |  点线  |
|  -.   | 点画线 |
|  --   | 短划线 |
|   -   |  实线  |

### color
| 符号  |     颜色      |
| :---: | :-----------: |
|   c   |   cyan青色    |
|   r   |    red红色    |
|   g   |   green绿色   |
|   b   |   blue蓝色    |
|   w   |   white白色   |
|   k   |   black黑色   |
|   y   |  yellow黄色   |
|   m   | magenta洋红色 |
[参考文献](https://zhuanlan.zhihu.com/p/258106097)

## 设置刻度
刻度的尺度
```python
x_tricks = np.linspace(-1, 3, 5)
plt.xtricks(x_tricks)
```
将x轴设定为数组"linspace(-1,3,5)"

设定刻度为指定显示
```python
plt.yticks([60,70,80,90], 
            [r'$及格$', r'$良好$', r'$不错$', r'$优秀$'])
```
其中在'$$'中,支持,mathjax符号

## 边框(坐标轴)颜色
```python
ax = plt.gca() # 获取当前的坐标轴
ax.spines['right'].set_color('red') # 设置右侧坐标轴为红色
ax.spines['top'].set_color('red') # 设置顶部坐标轴为红色
# ax.spines['top'].set_color('none')
```

## 设置刻度显示的位置
```python
ax.xaxis.set_ticks_position('bottom')
```
参数: bottom, top, both, default, none

## 设置坐标轴的位置
```python
ax.spines['bottom'].set_position(('data', 0))
# 设置底部的坐标轴(X轴)位置为刻度0点处
ax.spines['left'].set_position(('data', 0))
# 设置左侧的坐标轴(Y轴)位置为刻度0点处
```

## Legend图例解释
```python
plt.legend(loc='upper right')
```
loc参数:  

|     best     | upper right  |    right     |
| :----------: | :----------: | :----------: |
|  upper left  |  lower left  | center left  |
| lower right  |    right     | center right |
| lower center | upper center |    center    |

## 标注点的直线
画出一条垂直于x轴的虚线
```python
# 绘制虚线
x0 = 1
y0 = 2*x0 + 1
plt.plot([x0, x0,], [0, y0,], 'k--', linewidth=2.5)
# 绘制点
plt.scatter([x0, ], [y0, ], s=50, color='b')
```

## 添加曲线的注释
```python
plt.annotate(r'$2x+1=%s$' % y0, xy=(x0, y0), xycoords='data', xytext=(+30, -30),
             textcoords='offset points', fontsize=16,
             arrowprops=dict(arrowstyle='->', connectionstyle="arc3,rad=.2"))
```

|            参数            |          作用          |
| :------------------------: | :--------------------: |
|      xycoords='data'       |  基于数据的值来选位置  |
|     xytext=(+30, -30)      |        标注位置        |
| textcoords='offset points' |        xy偏差值        |
|         arrowprops         | 图中箭头类型的一些设置 |

## 标注文字
```python
plt.text(-1, 5, r'$This\ is\ the\ some\ text. \mu\ \sigma_i\ \alpha_t$',
         fontdict={'size': 16, 'color': 'r'})
```
其中"-1,5"表示text显示的位置

## 刻度透明度
如果坐标轴的刻度被曲线遮盖,可以通过调整透明度来显示
```python
for label in ax.get_xticklabels() + ax.get_yticklabels():
    label.set_fontsize(12)
    # 在 plt 2.0.2 或更高的版本中, 设置 zorder 给 plot 在 z 轴方向排序
    label.set_bbox(dict(facecolor='white', edgecolor='None', alpha=0.7, zorder=2))
```

|          参数          |             作用             |
| :--------------------: | :--------------------------: |
| label.set_fontsize(12) |       重新调节字体大小       |
|        set_bbox        | 设置目的内容的透明度相关参数 |
|       facecolor        |        调节box前景色         |
|       edgecolor        | 设置边框( 本处设置边框为无)  |
|         alpha          |          设置透明度          |


## numpy.arange
函数返回一个有终点和起点的固定步长的排列
如[1,2,3,4,5]，起点是1，终点是6，步长为1。
**参数个数情况：**
np.arange()函数分为一个参数，两个参数，三个参数三种情况

1. 一个参数时:参数值为终点，起点取默认值0，步长取默认值1。
2. 两个参数时:第一个参数为起点，第二个参数为终点，步长取默认值1。
3. 三个参数时:第一个参数为起点，第二个参数为终点，第三个参数为步长。其中步长支持小数

# 图表类型
## 散点图
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/1/4_.png">
```python
import matplotlib.pyplot as plt
import numpy as np

n = 1024    # data size
X = np.random.normal(0, 1, n)  # 每一个点的X值
Y = np.random.normal(0, 1, n)  # 每一个点的Y值
T = np.arctan2(Y, X)  # for color value
plt.scatter(X, Y, s=75, c=T, alpha=.5)
plt.xlim(-1.5, 1.5)
x_ticks = np.linspace(-1.5, 1.5, 10)
plt.xticks(x_ticks)
y_ticks = np.linspace(-1.5, 1.5, 10)
plt.ylim(-1.5, 1.5)
plt.yticks(y_ticks)
plt.show()
```

|     参数      |                  作用                  |
| :-----------: | :------------------------------------: |
| random.normal | 生成服从标准正态分布的数据(EX=0, DX=1) |
|     s=75      |                size尺寸                |
|      c=T      |           color map颜色状态            |
|     alpha     |                 透明度                 |

## 柱状图
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/1/2_.png">
```python
n = 12
X = np.arange(n) # X为数组
# 均匀分布
Y1 = (1 - X / float(n)) * np.random.uniform(0.5, 1.0, n)
Y2 = (1 - X / float(n)) * np.random.uniform(0.5, 1.0, n)

plt.bar(X, +Y1, facecolor='#9999ff', edgecolor='white')
plt.bar(X, -Y2, facecolor='#ff9999', edgecolor='white')
# facecolor为柱条颜色
plt.xlim(-.5, n)
plt.xticks(())
plt.ylim(-1.25, 1.25)
plt.yticks(())
# 添加数值标识
for x, y in zip(X, Y1):
    # ha: horizontal alignment
    # va: vertical alignment
    plt.text(x + 0.4, y + 0.05, '%.2f' % y, ha='center', va='bottom')
for x, y in zip(X, Y2):
    # ha: horizontal alignment
    # va: vertical alignment
    plt.text(x + 0.4, -y - 0.05, '%.2f' % y, ha='center', va='top')
plt.show()
```
**plt.text 分别在柱体上方和下方添加数值**
 - **%.2f** 保留两位小数
 - **ha='center'** 横向居中对齐
 - **va='bottom'** 纵向底部（顶部）对齐

## Contours 等高线图
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/1/1_.png">
数据集即三维点 (x,y) 和对应的高度值，共有256个点。高度值使用一个 height function f(x,y) 生成。 x, y 分别是在区间 [-3,3] 中均匀分布的256个值，并用meshgrid在二维平面中将每一个x和每一个y分别对应起来，编织成栅格:
```python
def f(x, y):
    # the height function
    return (1 - x / 2 + x**5 + y**3) * np.exp(-x**2 - y**2)
n = 256
x3 = np.linspace(-3, 3, n)
y3 = np.linspace(-3, 3, n)
X3, Y3 = np.meshgrid(x3, y3)
# use plt.contourf to filling contours
# X, Y and value for (X,Y) point
plt.contourf(X3, Y3, f(X3, Y3), 8, alpha=.75, cmap=plt.cm.hot)
# use plt.contour to add contour lines
C = plt.contour(X3, Y3, f(X3, Y3), 8, colors='black')
plt.clabel(C, inline=True, fontsize=10)
plt.xticks(())
plt.yticks(())
```

**颜色填充函数 plt.contourf**
 - 位置参数分别为：X, Y, f(X,Y)
 - 透明度0.75
 - 将 f(X,Y) 的值对应到color map的暖色组中寻找对应颜色

**等高线画线函数 plt.contour**
 - 位置参数为：X, Y, f(X,Y)
 - 颜色为黑色
 - 线条宽度为0.5

**添加高度数字函数 plt.clabel**
 - inline控制是否将Label画在线里面
 - 字体大小为10

## colorbar
显示数值参考颜色条
```python
# shrink:透明度
plt.colorbar(shrink=.92)
```
[参考文献-1](https://www.jianshu.com/p/d97c1d2e274f)
[参考文献-2](https://blog.csdn.net/sinat_32570141/article/details/105345725)

## 3D函数图像
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Matplotlib/1/0_.png">
```python
fig = plt.figure()
ax = Axes3D(fig)
# X, Y value
X = np.arange(-4, 4, 0.25)
Y = np.arange(-4, 4, 0.25)
X, Y = np.meshgrid(X, Y)    # x-y 平面的网格
R = np.sqrt(X ** 2 + Y ** 2)
# height value
Z = np.sin(R)
ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap=plt.get_cmap('rainbow'))
ax.contourf(X, Y, Z, zdir='y', offset=-4, cmap=plt.get_cmap('rainbow'))
```

|     参数     |      作用      |
| :----------: | :------------: |
| plot_surface |    绘制表面    |
|   rstride    | 表面纵向稀疏度 |
|   cstride    | 表面横向稀疏度 |
|     cmap     |    颜色样式    |
|   contourf   |      投影      |
|     zdir     |   投影面选择   |
|    offset    |    投影坐标    |

[numpy.meshgrid 函数解释](https://blog.csdn.net/lllxxq141592654/article/details/81532855)

# code
```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

x = np.linspace(-1, 3, 50)
y1 = 2*x + 1
y2 = x**2
plt.figure()
plt.plot(x, y1, label="line-1")
plt.plot(x, y2, color='red', linewidth=1.0, linestyle=':', label="line-2")
# plt.xlim((-1, 3))  # 设定X轴的坐标范围
# plt.ylim((-2, 7))  # 设定Y轴的坐标范围
# 创建坐标轴刻度数据
x_ticks = np.linspace(-1, 3, 5)
y_ticks = np.linspace(-2, 7, 10)
plt.xticks(x_ticks)
plt.yticks(y_ticks)
'''
# 设定Y坐标轴为特定符号
plt.yticks([-1, 0, 1, 2],
           [r'$\alpha$', r'$\beta$', r'$c$', r'$d$'])
'''
# 获取当前的坐标系
ax = plt.gca()
# 设置刻度线显示位置
ax.xaxis.set_ticks_position('top')
# 设定坐标轴的文字
plt.xlabel('I am x')  # X轴的名称
plt.ylabel('I am y')  # Y轴的名称
# 设定坐标轴为指定颜色
ax.spines['right'].set_color('red')
ax.spines['top'].set_color('red')
# 设定坐标轴位置
ax.spines['bottom'].set_position(('data', -2))
# 垂直坐标轴指示点-虚线
x0 = 1
y0 = 2*x0 + 1
plt.plot([x0, x0, ], [-2, y0, ], 'k--', linewidth=2.5)
# 垂直坐标轴指示点-绘制点
plt.scatter([x0, ], [y0, ], s=50, color='b')
# 曲线注释
plt.annotate(r'$2x+1=%s$' % y0, xy=(x0, y0), xycoords='data', xytext=(+30, -30),
             textcoords='offset points', fontsize=16,
             arrowprops=dict(arrowstyle='->', connectionstyle="arc3,rad=.2"))
# 标注文字
plt.text(-1, 5, r'$This\ is\ the\ some\ text. \mu\ \sigma_i\ \alpha_t$',
         fontdict={'size': 16, 'color': 'r'})
# 显示指示框
plt.legend(loc="best")

# 创建散点图
plt.figure()
n = 1024    # data size
X = np.random.normal(0, 1, n)  # 每一个点的X值
Y = np.random.normal(0, 1, n)  # 每一个点的Y值
T = np.arctan2(Y, X)  # for color value
plt.scatter(X, Y, s=75, c=T, alpha=.5)
plt.xlim(-1.5, 1.5)
x_ticks = np.linspace(-1.5, 1.5, 10)
plt.xticks(x_ticks)
y_ticks = np.linspace(-1.5, 1.5, 10)
plt.ylim(-1.5, 1.5)
plt.yticks(y_ticks)

# 创建柱状图
plt.figure()
n = 12
X = np.arange(n)  # X为数组
# 均匀分布
Y1 = (1 - X / float(n)) * np.random.uniform(0.5, 1.0, n)
Y2 = (1 - X / float(n)) * np.random.uniform(0.5, 1.0, n)

plt.bar(X, +Y1, facecolor='#9999ff', edgecolor='white')
plt.bar(X, -Y2, facecolor='#ff9999', edgecolor='white')
# facecolor为柱条颜色
plt.xlim(-.5, n)
plt.xticks(())
plt.ylim(-1.25, 1.25)
plt.yticks(())
# 添加数值标识
for x, y in zip(X, Y1):
    # ha: horizontal alignment
    # va: vertical alignment
    plt.text(x + 0.4, y + 0.05, '%.2f' % y, ha='center', va='bottom')
for x, y in zip(X, Y2):
    # ha: horizontal alignment
    # va: vertical alignment
    plt.text(x + 0.4, -y - 0.05, '%.2f' % y, ha='center', va='top')

# 绘制等高线
plt.figure()
def f(x, y):
    # the height function
    return (1 - x / 2 + x**5 + y**3) * np.exp(-x**2 - y**2)

n = 256
x3 = np.linspace(-3, 3, n)
y3 = np.linspace(-3, 3, n)
X3, Y3 = np.meshgrid(x3, y3)
# use plt.contourf to filling contours
# X, Y and value for (X,Y) point
plt.contourf(X3, Y3, f(X3, Y3), 8, alpha=.75, cmap=plt.cm.hot)
plt.colorbar(shrink=.92)
# use plt.contour to add contour lines
C = plt.contour(X3, Y3, f(X3, Y3), 8, colors='black')
plt.clabel(C, inline=True, fontsize=10)
plt.xticks(())
plt.yticks(())

# 绘制3D图像
fig = plt.figure()
ax = Axes3D(fig)
# X, Y value
X = np.arange(-4, 4, 0.25)
Y = np.arange(-4, 4, 0.25)
X, Y = np.meshgrid(X, Y)    # x-y 平面的网格
R = np.sqrt(X ** 2 + Y ** 2)
# height value
Z = np.sin(R)
ax.plot_surface(X, Y, Z, rstride=1, cstride=1, cmap=plt.get_cmap('rainbow'))
ax.contourf(X, Y, Z, zdir='y', offset=-4, cmap=plt.get_cmap('rainbow'))

plt.show()
```
