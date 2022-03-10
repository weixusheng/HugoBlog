# Pyside2开发第一个GUI程序


monitor_control
<!--more-->

# 起源
**Demo灵感来源**
[<font color="red">『又来瞎鼓捣』外接显示器不能自动调亮度？我花 10 块钱做了个自动光感调节器 - 少数派</font>](https://sspai.com/post/55797)

**Demo项目地址**
[<font color="red">GitHub</font>](https://github.com/weixusheng/monitor_control_GUI)

>调节外接显示器的亮度以及对比度

当时正在重温Python,正愁没啥代码可写,正好借着这个机会学学GUI开发,岂不美哉!
然后正好一直很眼馋跨平台开发,发现Qt这个好东西:smile:!
最后断断续续用了五天的时间,把这个程序写了出来,并且实现程序打包,可以在非开发(无python)环境下的电脑正常运行...嚯嚯嚯非常的开心了

# 开发环境
## 依赖库版本
|      name      | version |
| :------------: | :-----: |
|     Python     |  3.8.5  |
|    pyside2     | 5.15.2  |
| monitorcontrol |  2.3.0  |

一开始本来使用pyqt5去做,后来发现pyside2以及现在刚出的pyside6是官方支持的,以后兼容性以及生态会更好,因此这次直接选择用pyside2去搭建这个程序

## 开发工具
 - pycharm
 - qt-designer

## 配置pycharm

[<font color="red">参考博客</font>](https://www.cnblogs.com/agent/archive/2019/05/28/10816913.html)
配置成功后,在External tools中会有三个工具,依次为打开designer,ui文件转换为py文件,以及资源转换工具
在开发的第一个程序中,我们直接使用ui文件并且没有额外的资源调用,因此只会用到designer

# 界面设计
qt-designer步骤
1. 直接创建widget即可
2. 然后多选控件,进行布局(套娃)操作
3. 通过选择工具栏的信号/槽选项,然后拖拽控件,可以实现自带信号/槽函数的响应实现
4. 保存文件,放入程序的根目录下

在本程序中,信号/槽函数都在程序的脚本文件中定义,没有在ui文件中指定

# 程序脚本框架
# import部分
```python
from PySide2.QtWidgets import QApplication, QSystemTrayIcon, QMenu, QAction
from PySide2.QtUiTools import QUiLoader
from PySide2.QtCore import Signal, QObject, Qt
from PySide2.QtGui import QIcon
from threading import Thread
from monitorcontrol import get_monitors
import time
```
对于import部分,要使用
```python
from ... import ...
```
这样可以减少程序生成时,所依赖的包,大幅度减少程序打包后的大小

# 窗口类

[<font color="red">参考B站视频</font>](https://www.bilibili.com/video/BV1cJ411R7bP)
在脚本中,可以将一个窗口看作是一个类
通过类中的构造函数,初始化窗口中的参数以及绑定事件(信号/槽函数)

1. 引入ui文件
2. 初始化ui中参数
3. 绑定ui界面的事件(信号/槽函数)
4. 槽函数定义

## 引入ui/绑定事件
```python
class Stats():
    def __init__(self):
        # 引入ui文件
        self.ui = QUiLoader().load('main_ui.ui')
        # 获得当前屏幕亮度以及对比度
        with global_monitor[0]:
            s_contrast = global_monitor[0].get_contrast()
            s_luminance = global_monitor[0].get_luminance()
        # 初始化划轴位置
        self.ui.horizontalSlider.setValue(s_luminance)
        self.ui.horizontalSlider_2.setValue(s_contrast)
        # 初始化显示
        self.ui.lcdNumber.display(s_luminance)
        self.ui.lcdNumber_2.display(s_contrast)
        # 建立信号量实例(子线程改变主界面,防止画面阻塞)
        self.global_ms = MySignals()
        # 绑定事件
        self.ui.pushButton.clicked.connect(self.minimize_totray)
        self.ui.horizontalSlider.valueChanged.connect(lambda: self.update_shit(1))
        self.ui.horizontalSlider_2.valueChanged.connect(lambda: self.update_shit(2))
        self.global_ms.main_signal.connect(self.main_thread)
```

## 槽函数定义
其实就是事件的响应函数,需要注意的是,在信号的绑定中,如果需要传递参数,那么需要添加"lambda"关键字,具体原理还不知道,先暂时这样记吧
```python
self.ui.horizontalSlider_2.valueChanged.connect(lambda: self.update_shit(2))
```
**函数完整定义**
```python
    def main_thread(self, index, value):
        global last_time
        now_time = time.time()
        global global_monitor
        if now_time-last_time > 0.1:
            if index == 0:
                self.ui.lcdNumber.display(value)
                with global_monitor[0]:
                    global_monitor[0].set_luminance(value)
                last_time = now_time
            else:
                self.ui.lcdNumber_2.display(value)
                with global_monitor[0]:
                    global_monitor[0].set_contrast(value)
                last_time = now_time
        else:
            pass
```

## 画面阻塞处理(子线程)
[<font color="red">参考博客</font>](http://www.python3.vip/tut/py/gui/qt_08/)
如果在脚本中,需要进行函数操作,之后更新到ui上,但是函数操作需要较长的时间,那么就会出现画面阻塞的情况,即此时画面卡死,去等待函数操作完成,这个情况在gui开发中,会经常出现,最好的解决方式就是调用子线程,而在子线程中更新ui,需要用到python的signal机制

1. 首先需要创建一个signal类
    ```python
    class MySignals(QObject):
        main_signal = Signal(int, int)
    ```

2. 然后再主窗口类实例化,并绑定
    ```python
    self.global_ms = MySignals()
    self.global_ms.main_signal.connect(self.main_thread)
    ```

3. 定义处理函数
    ```python
    def main_thread(self, index, value):
        # command...  
    ```

4. 将ui上的动作与信号绑定
   ```python
    def update_shit(self, shit):
        def threadFunc():
            if shit == 1:
                new_data = self.ui.horizontalSlider.value()
                self.global_ms.main_signal.emit(0, new_data)
            else:
                new_data = self.ui.horizontalSlider_2.value()
                self.global_ms.main_signal.emit(1, new_data)
        thread = Thread(target=threadFunc)
        thread.start()   
   ```
   其中emit,就是发送信号值
   

**emit信号发送到"global_ms"实例对象,然后"global_ms"去调用绑定的"main_thread"函数,最后完成了子线程调用主线程函数,从而不会发生主线程等待**

# 托盘实现
## 构造函数
如果想将程序隐藏至托盘,那么需要新定义一个托盘类
```python
class TrayIcon(QSystemTrayIcon):
    def __init__(self, MainWindow, parent=None):
        super(TrayIcon, self).__init__(parent)
        self.ui = MainWindow
        self.createMenu()
```
构造函数传参为主窗口类的实例化对象

## 创建托盘右键菜单
```python
def createMenu(self):
    self.menu = QMenu()
    self.showAction1 = QAction("打开", self, triggered=self.show_window)
    self.showAction2 = QAction("夜晚模式", self, triggered=self.open_night)
    self.showAction3 = QAction("白天模式", self, triggered=self.open_light)
    self.quitAction = QAction("退出", self, triggered=self.quitapp)
    # 托盘增加选项
    self.menu.addAction(self.showAction1)
    self.menu.addAction(self.showAction2)
    self.menu.addAction(self.showAction3)
    self.menu.addAction(self.quitAction)
    self.setContextMenu(self.menu)
    # 设置图标
    self.setIcon(QIcon("star.png"))
    self.icon = self.MessageIcon()
    # 把鼠标点击图标的信号和槽连接
    self.activated.connect(self.onIconClicked)
```

## 定义菜单绑定事件
```python
def open_night(self):
    # 系统提示弹窗
    self.showMessage("Message", "已开启夜晚模式", self.icon)
    # 实现过程(调节亮度,更新ui界面数值)
    global global_monitor
    with global_monitor[0]:
        global_monitor[0].set_contrast(50)
        global_monitor[0].set_luminance(35)
    self.ui.horizontalSlider.setValue(35)
    self.ui.horizontalSlider_2.setValue(50)
    self.ui.lcdNumber.display(35)
    self.ui.lcdNumber_2.display(50)
```

## 隐藏至托盘
在**主窗口函数**的事件中定义
```python
def minimize_totray(self):
    self.ui.setWindowFlags(Qt.SplashScreen | Qt.FramelessWindowHint)
    self.ui.showMinimized()
```

## 点击托盘显示窗口/退出程序
```python
def show_window(self):
    # 若是最小化，则先正常显示窗口，再变为活动窗口（暂时显示在最前面）
    self.ui.showNormal()
    self.ui.activateWindow()
    self.ui.setWindowFlags(Qt.Window)
    self.ui.show()

def quitapp(self):
    QApplication.quit()
```

## 定义点击托盘图标事件
鼠标点击icon传递的信号会带有一个整形的值
1. 表示单击右键
2. 双击
3. 单击左键
4. 用鼠标中键点击


```python
def onIconClicked(self, reason):
    if reason == 2 or reason == 3:
        # self.showMessage("Message", "skr at here", self.icon)
        if self.ui.isMinimized() or not self.ui.isVisible():
            # 若是最小化，则先正常显示窗口，再变为活动窗口（暂时显示在最前面）
            self.ui.showNormal()
            self.ui.activateWindow()
            self.ui.setWindowFlags(Qt.Window)
            self.ui.show()
        else:
            # 若不是最小化，则最小化
            self.ui.showMinimized()
            self.ui.setWindowFlags(Qt.SplashScreen)
            self.ui.show()
```

# 测试当前窗口
适合于测试当前窗口程序,在完成测试之后应该删去或注释
```python
if __name__ == "__main__":  # 用于当前窗体测试
    app = QApplication([])
    app.setWindowIcon(QIcon('star.png'))
    #实例化主程序
    stats = Stats()
    stats.ui.show()
    # 实例化托盘程序
    ti = TrayIcon(stats.ui)
    ti.show()
    app.exec_()
```

# 窗口与运行的分离
新创建一个"main_run.py"文件,将所有窗口类在该文件中import
然后控制窗口调用,而不是将所有窗口写在一个文件中
(由于Demo只有一个窗口吗,所以看起来没啥区别...
```python
from PySide2.QtGui import QIcon
from PySide2.QtWidgets import QApplication
from main_script import Stats, TrayIcon

app = QApplication([])
app.setWindowIcon(QIcon('star.png'))
# 实例化主程序
stats = Stats()
stats.ui.show()
# 实例化托盘程序
ti = TrayIcon(stats.ui)
ti.show()
app.exec_()
```

这样第一个Demo就完成啦
Python太强啦,没想到开发GUI这么简单!!!好快乐
