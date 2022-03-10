# Python多线程



多线程 / 线程池
<!--more-->

# pyhton多线程

## Python多线程的默认情况

当一个进程启动之后，会默认产生一个主线程，因为线程是程序执行流的最小单元，当设置多线程时，主线程会创建多个子线程，在python中，默认情况下（其实就是setDaemon(False)），主线程执行完自己的任务以后，就退出了，此时子线程会继续执行自己的任务，直到自己的任务结束。

```python
import threading
import time

def run():
    time.sleep(2)
    print("当前线程的名字是: %s -" %threading.current_thread().name)
    time.sleep(2)


if __name__ == '__main__':

    start_time = time.time()

    print('这是主线程：', threading.current_thread().name)
    thread_list = []
    for i in range(5):
        t = threading.Thread(target=run)
        thread_list.append(t)

    for t in thread_list:
        t.start()

    print('主线程结束！' , threading.current_thread().name)
    print('一共用时：', time.time()-start_time)
    
""" output:
这是主线程： MainThread
主线程结束！ MainThread
一共用时： 0.0020024776458740234
当前线程的名字是: Thread-49 -
当前线程的名字是: Thread-53 -
当前线程的名字是: Thread-52 -
当前线程的名字是: Thread-51 -
当前线程的名字是: Thread-50 -
"""
```

1. 我们的计时是对主线程计时，主线程结束，计时随之结束，打印出主线程的用时。
2. 主线程的任务完成之后，主线程随之结束，子线程继续执行自己的任务，直到全部的子线程的任务全部结束，程序结束

## 设置守护线程

当我们使用setDaemon(True)方法，设置子线程为守护线程时，主线程一旦执行结束，则全部线程全部被终止执行，可能出现的情况就是，子线程的任务还没有完全执行结束，就被迫停止。

```python
import threading
import time

def run():

    time.sleep(2)
    print('当前线程的名字是： ', threading.current_thread().name)
    time.sleep(2)


if __name__ == '__main__':

    start_time = time.time()

    print('这是主线程：', threading.current_thread().name)
    thread_list = []
    for i in range(5):
        t = threading.Thread(target=run)
        thread_list.append(t)

    for t in thread_list:
        t.setDaemon(True)
        t.start()

    print('主线程结束了！' , threading.current_thread().name)
    print('一共用时：', time.time()-start_time)
    
""" output
这是主线程： MainThread
主线程结束了！ MainThread
一共用时： 0.0020020008087158203
"""
```



主线程结束以后，子线程还没有来得及执行，整个程序就退出了

## join的用法

此时join的作用就凸显出来了，join所完成的工作就是线程同步，即主线程任务结束之后，进入阻塞状态，一直等待其他的子线程执行结束之后，主线程在终止。

```python
import threading
import time

def run():

    time.sleep(2)
    print('当前线程的名字是： ', threading.current_thread().name)
    time.sleep(2)


if __name__ == '__main__':

    start_time = time.time()

    print('这是主线程：', threading.current_thread().name)
    thread_list = []
    for i in range(5):
        t = threading.Thread(target=run)
        thread_list.append(t)

    for t in thread_list:
        t.setDaemon(True)
        t.start()

    for t in thread_list:
        t.join()

    print('主线程结束了！' , threading.current_thread().name)
    print('一共用时：', time.time()-start_time)
"""
这是主线程： MainThread
当前线程的名字是：  Thread-85
当前线程的名字是：  Thread-84
当前线程的名字是：  Thread-86
当前线程的名字是：  Thread-88
当前线程的名字是：  Thread-87
主线程结束了！ MainThread
一共用时： 4.0279927253723145
"""
```

主线程一直等待全部的子线程结束之后，主线程自身才结束，程序退出

## 实践

```python
import threading

threads = []
for k, record in enumerate(data['supplyname']):
	# get_list # get_page
	thread_spider = None
	if method == 'get_list':
		thread_spider = threading.Thread(target=get_list, args=[url, ws_data, record, json_list])
	elif method == 'get_page':
		thread_spider = threading.Thread(target=get_page, args=[url, ws_data, record, json_list])
	thread_spider.start()
	threads.append(thread_spider)
	print('当前进度: %s / %s' % (k + 1, str(data_num)))
for t in threads:
	t.join()
```

# 线程池

## 创建线程池

之前用信号量,感觉不太好用...发现多线程自带一个线程池功能

`concurrent.futures`模块，它提供了`ThreadPoolExecutor`和`ProcessPoolExecutor`两个类，实现了对`threading`和`multiprocessing`的进一步抽象（这里主要关注线程池），不仅可以帮我们***自动调度线程\***，还可以做到：

1. 主线程可以获取某一个线程（或者任务的）的状态，以及返回值。

2. 当一个线程完成的时候，主线程能够立即知道。

3. 让多线程和多进程的编码接口一致。

```python
from concurrent.futures import ThreadPoolExecutor
import time

# 参数times用来模拟网络请求的时间
def get_html(times):
    time.sleep(times)
    print("get page {}s finished".format(times))
    return times

executor = ThreadPoolExecutor(max_workers=2)
# 通过submit函数提交执行的函数到线程池中，submit函数立即返回，不阻塞
task1 = executor.submit(get_html, (3))
task2 = executor.submit(get_html, (2))
# done方法用于判定某个任务是否完成
print(task1.done())
# cancel方法用于取消某个任务,该任务没有放入线程池中才能取消成功
print(task2.cancel())
time.sleep(4)
print(task1.done())
# result方法可以获取task的执行结果
print(task1.result())
```

   ## 利用as_completed取出所有结果

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import time

# 参数times用来模拟网络请求的时间
def get_html(times):
    time.sleep(times)
    print("get page {}s finished".format(times))
    return times

executor = ThreadPoolExecutor(max_workers=2)
urls = [3, 2, 4] # 并不是真的url
all_task = [executor.submit(get_html, (url)) for url in urls]

for future in as_completed(all_task):
    data = future.result()
    print("in main: get page {}s success".format(data))
```

`as_completed()`方法是一个生成器，在没有任务完成的时候，会阻塞，在有某个任务完成的时候，会`yield`这个任务，就能执行for循环下面的语句，然后继续阻塞住，循环到所有的任务结束。从结果也可以看出，**先完成的任务会先通知主线程**

## 通过map提交任务

```python
from concurrent.futures import ThreadPoolExecutor
import time

# 参数times用来模拟网络请求的时间
def get_html(times):
    time.sleep(times)
    print("get page {}s finished".format(times))
    return times

executor = ThreadPoolExecutor(max_workers=2)
urls = [3, 2, 4] # 并不是真的url

for data in executor.map(get_html, urls):
    print("in main: get page {}s success".format(data))
# 执行结果
# get page 2s finished
# get page 3s finished
# in main: get page 3s success
# in main: get page 2s success
# get page 4s finished
# in main: get page 4s success

'''
使用map方法，无需提前使用submit方法，map方法与python标准库中的map含义相同，都是将序列中的每个元素都执行同一个函数。上面的代码就是对urls的每个元素都执行get_html函数，并分配各线程池。可以看到执行结果与上面的as_completed方法的结果不同，输出顺序和urls列表的顺序相同，就算2s的任务先执行完成，也会先打印出3s的任务先完成，再打印2s的任务完成。
'''
```

## 多参数传入submit

```python
from concurrent.futures import ThreadPoolExecutor,as_completed
 
 
def doFileParse(filepath,segment,wordslist):
      print(filepath)
      print(segment)
 
#调用方法
#实质就是通过lambda表达式过渡。传入的参数是一个，但是通过lambda表达多后拆散为多个传入。这是很巧妙的方法，实际 就是 *p 这个表达式。
args =[filepath,thu1,Words]
 
newTask=executor.submit(lambda p: doFileParse(*p),args)
```

# 临界资源互斥访问

threading 模块提供了 Lock 类

- acquire()：对 Lock加锁，其中timeout参数指定加锁多少秒
- release()：释放锁

RLock被称为重入锁，可以被一个线程请求多次，即锁中可以嵌套锁

## 单锁

```python
class Account:
    def __init__(self, card_id, balance):
        # 封装账户ID、账户余额的两个变量
        self.card_id= card_id
        self.balance = balance
        
def withdraw(account, money):
    # 进行加锁
    lock.acquire()
    # 账户余额大于取钱数目
    if account.balance >= money:
        # 吐出钞票
        print(threading.current_thread().name + "取钱成功！吐出钞票:" + str(money),end=' ')
        # 修改余额
        account.balance -= money
        print("\t余额为: " + str(account.balance))
    else:
        print(threading.current_thread().name + "取钱失败！余额不足")
    # 进行解锁
    lock.release()
# 创建一个账户，银行卡id为8888，存款1000元
acct = Account("8888" , 1000)

# 模拟两个对同一个账户取钱
# 在主线程中创建一把锁
lock = threading.Lock()
threading.Thread(name='窗口A', target=withdraw , args=(acct , 800)).start()
threading.Thread(name='窗口B', target=withdraw , args=(acct , 800)).start()
```

## 多重锁

```python
import threading
import time

def fun_1():
    print('开始')
    time.sleep(1)
    lock.acquire()
    print("第一道锁")
    fun_2()
    lock.release()
    
def fun_2():
    lock.acquire()
    print("第二道锁")
    lock.release()
    
if __name__ == '__main__':
    lock = threading.RLock()
    t1 = threading.Thread(target=fun_1)
    t2 = threading.Thread(target=fun_1)
    t1.start()
    t2.start()
```






