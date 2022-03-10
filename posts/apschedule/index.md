# APScheduler的使用


定时任务框架的使用
<!--more-->


# 安装

```bash
pip install apscheduler
```

# 基本概念介绍

**触发器（triggers）**：触发器包含调度逻辑，描述一个任务何时被触发，按日期或按时间间隔或按 cronjob 表达式三种方式触发。每个作业都有它自己的触发器，除了初始配置之外，触发器是完全无状态的。

**作业存储器（job stores）**：作业存储器指定了作业被存放的位置，默认情况下作业保存在内存，也可将作业保存在各种数据库中，当作业被存放在数据库中时，它会被序列化，当被重新加载时会反序列化。作业存储器充当保存、加载、更新和查找作业的中间商。在调度器之间不能共享作业存储。

**执行器（executors）**：执行器是将指定的作业（调用函数）提交到线程池或进程池中运行，当任务完成时，执行器通知调度器触发相应的事件。

**调度器（schedulers）**：任务调度器，属于控制角色，通过它配置作业存储器、执行器和触发器，添加、修改和删除任务。调度器协调触发器、作业存储器、执行器的运行，通常只有一个调度程序运行在应用程序中，开发人员通常不需要直接处理作业存储器、执行器或触发器，配置作业存储器和执行器是通过调度器来完成的。

# 任务实例

## 间隔性任务

```python
from apscheduler.schedulers.blocking import BlockingScheduler

def tick():
    print('Tick! The time is: %s' % datetime.now())

if __name__ == '__main__':
    scheduler = BlockingScheduler()
    scheduler.add_job(tick, 'interval', seconds=3)
    print('Press Ctrl+{0} to exit'.format('Break' if os.name == 'nt' else 'C    '))

    try:
        scheduler.start()
    except (KeyboardInterrupt, SystemExit):
        pass
```

导入调度器模块 `BlockingScheduler`，这是最简单的调度器，调用 `start` 方阻塞当前进程

如果程序只用于调度，除了调度进程外没有其他后台进程，那么请用 `BlockingScheduler` 非常有用，此时调度进程相当于守护进程。

## cron 任务

```python
from apscheduler.schedulers.blocking import BlockingScheduler

def tick():
    print('Tick! The time is: %s' % datetime.now())

if __name__ == '__main__':
    scheduler = BlockingScheduler()
    scheduler.add_job(tick, 'cron', hour=19,minute=23)
    try:
        scheduler.start()
    except (KeyboardInterrupt, SystemExit):
        pass
```

定时 `cron`任务非常简单，直接给触发器 `trigger` 传入 `cron` 即可。`hour =19 ,minute =23` 这里表示每天的`19：23` 分执行任务。这里可以填写数字，也可以填写字符串

```python
hour =19 , minute =23
hour ='19', minute ='23'
minute = '*/3' 表示每 5 分钟执行一次
hour ='19-21', minute= '23' 表示 19:23、 20:23、 21:23 各执行一次任务
```

# 配置调度器

调度器的主循环其实就是反复检查是不是有到时需要执行的任务，分以下几步进行：

- 询问自己的每一个作业存储器，有没有到期需要执行的任务，如果有，计算这些作业中每个作业需要运行的时间点，如果时间点有多个，做 coalesce 检查。

- 提交给执行器按时间点运行。

**各调度器的适用场景：**

- BlockingScheduler：适用于调度程序是进程中唯一运行的进程，调用start函数会阻塞当前线程，不能立即返回。
- BackgroundScheduler：适用于调度程序在应用程序的后台运行，调用start后主线程不会阻塞。
- AsyncIOScheduler：适用于使用了asyncio模块的应用程序。
- GeventScheduler：适用于使用gevent模块的应用程序。
- TwistedScheduler：适用于构建Twisted的应用程序。
- QtScheduler：适用于构建Qt的应用程序。
  上述调度器可以满足我们绝大多数的应用环境，本文以两种调度器为例说明如何进行调度器配置。

作业存储器的选择有两种：

一是内存，也是默认的配置；二是数据库

具体选哪一种看我们的应用程序在崩溃时是否重启整个应用程序，如果重启整个应用程序，那么作业会被重新添加到调度器中，此时简单的选取内存作为作业存储器即简单又高效。但是，当调度器重启或应用程序崩溃时您需要您的作业从中断时恢复正常运行，那么通常我们选择将作业存储在数据库中

执行器的选择也取决于应用场景。通常默认的 ThreadPoolExecutor 已经足够好。如果作业负载涉及CPU 密集型操作，那么应该考虑使用 ProcessPoolExecutor，甚至可以同时使用这两种执行器，将ProcessPoolExecutor 行器添加为二级执行器。

apscheduler 提供了许多不同的方法来配置调度器。可以使用字典，也可以使用关键字参数传递。首先实例化调度程序，添加作业，然后配置调度器，获得最大的灵活性。

# 配置示例

- 配置名为“mongo”的MongoDBJobStore作业存储器
- 配置名为“default”的SQLAlchemyJobStore(使用SQLite)
- 配置名为“default”的ThreadPoolExecutor，最大线程数为20
- 配置名为“processpool”的ProcessPoolExecutor，最大进程数为5
- UTC作为调度器的时区
- coalesce默认情况下关闭
- 作业的默认最大运行实例限制为3

```python
from pytz import utc
from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.jobstores.mongodb import MongoDBJobStore
from apscheduler.jobstores.sqlalchemy import SQLAlchemyJobStore
from apscheduler.executors.pool import ThreadPoolExecutor, ProcessPoolExecutor

jobstores = {
       'default': SQLAlchemyJobStore(url='sqlite:///jobs.sqlite'),
       'shit': MongoDBJobStore()
}
executors = {
       'default': ThreadPoolExecutor(20),
       'shit': ProcessPoolExecutor(5)
}
job_defaults = {
       'coalesce': False,
       'max_instances': 3
}
scheduler = BackgroundScheduler(jobstores=jobstores,executors=executors,job_defaults=job_defaults,timezone=utc) 
```

## 增加任务`add_job`

[文档的参数描述](https://apscheduler.readthedocs.io/en/latest/modules/schedulers/base.html#apscheduler.schedulers.base.BaseScheduler.add_job)

`add_job`(func, trigger=None, args=None, kwargs=None, id=None, name=None, misfire_grace_time=undefined, coalesce=undefined, max_instances=undefined, next_run_time=undefined, jobstore='default', executor='default', replace_existing=False, **trigger_args)

```python
scheduler.add_job(func_name, trigger, args=[...],
                  id="this_is_id",
                  jobstore="shit", # 使用先前声明中存在的配置
                  executor="shit" # 使用先前声明中存在的配置
                 )
```

## 触发器类型 `trigger types`:

- [`date`](https://apscheduler.readthedocs.io/en/latest/modules/triggers/date.html#module-apscheduler.triggers.date): use when you want to run the job just once at a certain point of time
- [`interval`](https://apscheduler.readthedocs.io/en/latest/modules/triggers/interval.html#module-apscheduler.triggers.interval): use when you want to run the job at fixed intervals of time
- [`cron`](https://apscheduler.readthedocs.io/en/latest/modules/triggers/cron.html#module-apscheduler.triggers.cron): use when you want to run the job periodically at certain time(s) of day

# 时间设定

## `corn`定时调度

```python
(int|str) 表示参数既可以是int类型，也可以是str类型
(datetime | str) 表示参数既可以是datetime类型，也可以是str类型
 
year (int|str) – 4-digit year -（表示四位数的年份，如2008年）
month (int|str) – month (1-12) -（表示取值范围为1-12月）
day (int|str) – day of the (1-31) -（表示取值范围为1-31日）
week (int|str) – ISO week (1-53) -（格里历2006年12月31日可以写成2006年-W52-7（扩展形式）或2006W527（紧凑形式））
day_of_week (int|str) – number or name of weekday (0-6 or mon,tue,wed,thu,fri,sat,sun) - （表示一周中的第几天，既可以用0-6表示也可以用其英语缩写表示）
hour (int|str) – hour (0-23) - （表示取值范围为0-23时）
minute (int|str) – minute (0-59) - （表示取值范围为0-59分）
second (int|str) – second (0-59) - （表示取值范围为0-59秒）
start_date (datetime|str) – earliest possible date/time to trigger on (inclusive) - （表示开始时间）
end_date (datetime|str) – latest possible date/time to trigger on (inclusive) - （表示结束时间）
timezone (datetime.tzinfo|str) – time zone to use for the date/time calculations (defaults to scheduler timezone) -（表示时区取值）
```

### 示例

```python
#表示2017年3月22日17时19分07秒执行该程序
sched.add_job(my_job, 'cron', year=2017,month = 03,day = 22,hour = 17,minute = 19,second = 07)
 
#表示任务在6,7,8,11,12月份的第三个星期五的00:00,01:00,02:00,03:00 执行该程序
sched.add_job(my_job, 'cron', month='6-8,11-12', day='3rd fri', hour='0-3')
 
#表示从星期一到星期五5:30（AM）直到2014-05-30 00:00:00
sched.add_job(my_job(), 'cron', day_of_week='mon-fri', hour=5, minute=30,end_date='2014-05-30')
 
#表示每5秒执行该程序一次，相当于interval 间隔调度中seconds = 5
sched.add_job(my_job, 'cron',second = '*/5')
```

## `interval`间隔调度

```python
weeks (int) – number of weeks to wait
days (int) – number of days to wait
hours (int) – number of hours to wait
minutes (int) – number of minutes to wait
seconds (int) – number of seconds to wait
start_date (datetime|str) – starting point for the interval calculation
end_date (datetime|str) – latest possible date/time to trigger on
timezone (datetime.tzinfo|str) – time zone to use for the date/time calculations
```

### 示例

```python
#表示每隔3天17时19分07秒执行一次任务
sched.add_job(my_job, 'interval',days  = 03,hours = 17,minutes = 19,seconds = 07)
```

## `date` 定时调度（作业只会执行一次)

```python
run_date (datetime|``str``) – the date``/``time to run the job at ``-``（任务开始的时间）
timezone (datetime.tzinfo|``str``) – time zone ``for` `run_date ``if` `it doesn’t have one already
```

### 示例

```python
# The job will be executed on November 6th, 2009
sched.add_job(my_job, 'date', run_date=date(2009, 11, 6), args=['text'])
# The job will be executed on November 6th, 2009 at 16:30:05
sched.add_job(my_job, 'date', run_date=datetime(2009, 11, 6, 16, 30, 5), args=['text'])
```

# 常见问题

```python
job_defaults = {
    'coalesce': True,
    'misfire_grace_time': None
}
scheduler = BackgroundScheduler(job_defaults=job_defaults) # 全局设置misfire_grace_time
# 或者
scheduler.add_job(……, misfire_grace_time=None, coalesce=True) # 对单个任务设置 misfire_grace_time
```

​    每个任务都有一个misfire_grace_time，单位：秒，默认是0秒。意思是 那些错过的任务在有条件执行时（有线程空闲出来/服务已恢复），如果还没有超过misfire_grace_time，就会被再次执行。如果misfire_grace_time=None，就是不论任务错过了多长时间，都会再次执行。
 原文：

> misfire_grace_time: seconds after the designated runtime that the job is still allowed to be run (or `None` to allow the job to run no matter how late it is)

​    如果有个任务C是每2分钟执行一次的周期性任务，且设置了较长misfire_grace_time ,结果服务停了10分钟，10分钟后服务恢复，那么错过的5个任务C都会执行。这种情况下，如果你只想执行1个任务C，可以设置coalesce = True。

# 立即执行

To add a job to be run immediately:

```pyhton
# The 'date' trigger and datetime.now() as run_date are implicit
sched.add_job(my_job, args=['text'])
```


