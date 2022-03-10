# 行为型模式



PY设计模式
<!--more-->


# 责任链模式

责任链模式的内容：使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系。将这些对象连成一条链并沿着这条链传递该请求，直到有一个对象处理它为止。责任链的角色有抽象处理者、具体处理者和客户端。
```python
from abc import ABCMeta, abstractmethod

# 抽象的处理者
class Handler(metaclass=ABCMeta):
    @abstractmethod
    def handle_leave(self, day):
        pass

# 具体的处理者
class GeneralManager(Handler):
    def handle_leave(self, day):
        if day <= 30:
            print('总经理准假%d' % day)
        else:
            print('可以辞职了！')

# 具体的处理者
class DepartmentManager(Handler):
    def __init__(self):
        self.next = GeneralManager()

    def handle_leave(self, day):
        if day <= 7:
            print('项目主管准假%d' % day)
        else:
            print('部门经理职权不足')
            self.next.handle_leave(day)

# 具体的处理者
class ProjectDirector(Handler):
    def __init__(self):
        self.next = DepartmentManager()

    def handle_leave(self, day):
        if day <= 3:
            print('项目主管准假%d' % day)
        else:
            print('项目主管职权不足')
            self.next.handle_leave(day)

day = 20
p = ProjectDirector()
p.handle_leave(day)
"""
项目主管职权不足
部门经理职权不足
总经理准假20
"""
```
使用场景：有多个对象可以处理一个请求，哪个对象处理由运行时决定；在不明确接收者的情况下，向多个对象中的一个提交一个请求。优点是降低耦合度，一个对象无需知道是其它哪一个对象处理其请求。

# 观察者模式

观察者模式应用比较广泛，又被称为“发布-订阅”模式。它用来定义对象间一种一对多的依赖关系，当一个对象的状态发生变化时，所有依赖它的对象都得到通知并被自动更新。观察者模式的角色有：抽象主题、具体主题（发布者）、抽象观察者和具体观察者（订阅者）。
```python
from abc import ABCMeta, abstractmethod

# 抽象的订阅者
class Observer(metaclass=ABCMeta):
    @abstractmethod
    def update(self, notice):
        """
        :param notice: Notice类的对象
        :return:
        """
        pass

# 抽象的发布者：可以是接口，子类不需要实现，所以不需要定义抽象方法！
class Notice:
    def __init__(self):
        self.observers = []

    def attach(self, obs):
        self.observers.append(obs)

    def detach(self, obs):
        self.observers.remove(obs)

    def notify(self):
        """
        推送
        :return:
        """
        for obs in self.observers:
            obs.update(self)

# 具体的发布者
class StaffNotice(Notice):
    def __init__(self, company_info):
        super().__init__()  # 调用父类对象声明observers属性
        self.__company_info = company_info

    @property
    def company_info(self):
        return self.__company_info

    @company_info.setter
    def company_info(self, info):
        self.__company_info = info
        self.notify()

# 具体的订阅者
class Staff(Observer):
    def __init__(self):
        self.company_info = None

    def update(self, notice):
        self.company_info = notice.company_info

staff_notice = StaffNotice('初始化公司信息')
staff1 = Staff()
staff2 = Staff()
staff_notice.attach(staff1)
staff_notice.attach(staff2)
# print(staff1.company_info) None
# print(staff2.company_info) None
staff_notice.company_info = '假期放假通知！'
print(staff1.company_info)
print(staff2.company_info)
staff_notice.detach(staff2)
staff_notice.company_info = '明天开会！'
print(staff1.company_info)
print(staff2.company_info)
"""
假期放假通知！
假期放假通知！
明天开会！
假期放假通知！
"""
```
使用场景：当一个抽象模型有两个方面，其中一个方面依赖另一个方面。将这两者封装在独立对象中以使它们可以各自独立地改变和复用；当对一个对象的改变需要同时改变其它对象，而不知道具体有多少对象待改变；当一个对象必须通知其它对象，而它又不能假定其它对象是谁。换言之，你不希望这些对象是紧耦合的。优点：目标和观察者之间的抽象耦合最小；支持广播通信。

# 策略模式

定义一个个算法，把它们封装起来，并且使它们可以相互替换。本模式使得算法可独立于使用它的客户而变化。角色有：抽象策略、具体策略和上下文。
```python
from abc import abstractmethod, ABCMeta
from datetime import datetime

# 抽象策略
class Strategy(metaclass=ABCMeta):
    @abstractmethod
    def execute(self, data):
        pass

# 具体策略
class FastStrategy(Strategy):
    def execute(self, data):
        print("使用较快的策略处理%s" % data)

# 具体策略
class SlowStrategy(Strategy):
    def execute(self, data):
        print("使用较慢的策略处理%s" % data)

# 上下文
class Context:
    def __init__(self, strategy, data):
        self.data = data
        self.strategy = strategy
        # 可以定义用户不知道的东西
        self.date = datetime.now()

    def set_strategy(self, strategy):
        self.strategy = strategy

    def do_strategy(self):
        self.strategy.execute(self.data)

data = "Hello!"
# 使用较快的策略处理
fast_strategy = FastStrategy()
context = Context(fast_strategy, data)
context.do_strategy()
# 使用较慢的策略处理
slow_strategy = SlowStrategy()
context = Context(slow_strategy, data)
context.do_strategy()
"""
使用较快的策略处理Hello!
使用较慢的策略处理Hello!
"""
```
优点：定义了一些列可重用的算法和行为；消除了一些条件语句；可以提供相同行为的不同实现；缺点：客户必须了解不同的策略。

# 模板方法模式

内容：定义一个操作中的算法骨架，将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。使用模板方法，需要用到两种角色，分别是抽象类和具体类。抽象类的作用是是定义抽象类（钩子操作），实现一个模板方法作为算法的骨架。具体类的作用实现原子操作。
```python
from abc import ABCMeta, abstractmethod
from time import sleep

# 抽象类
class Window(metaclass=ABCMeta):
    @abstractmethod
    def start(self):  # 原子操作/钩子操作
        pass

    @abstractmethod
    def repaint(self):  # 原子操作/钩子操作
        pass

    @abstractmethod
    def stop(self):  # 原子操作/钩子操作
        pass

    def run(self):
        """
        模板方法(具体方法)，这个大逻辑就不需要自己写了
        :return:
        """
        self.start()
        while True:
            try:
                self.repaint()
                sleep(1)
            except KeyboardInterrupt:
                break
        self.stop()

# 具体类
class MyWindow(Window):
    def __init__(self, msg):
        self.msg = msg

    def start(self):
        print('窗口开始运行！')

    def stop(self):
        print('窗口停止运行！')

    def repaint(self):
        print(self.msg)

MyWindow("Hello...").run()
```
模板方法适用的场景：一次性实现一个算法的不变部分，各个子类中的公共行为应该被提取出来并集中到一个公共父类中以避免代码重复；控制子类扩展。
