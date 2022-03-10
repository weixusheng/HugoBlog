# 进度条显示tqdm



进度条tqdm的使用
<!--more-->

## 安装

```bash
pip install tqdm
```

## 迭代对象处理

对于可以`迭代的对象`都可以使用下面这种方式，来实现可视化进度，非常方便

```python
from tqdm import tqdm
import time
 
for i in tqdm(range(100)):
  time.sleep(0.1)
  pass
```

在使用`tqdm`的时候，可以将`tqdm(range(100))`替换为`trange(100)`

```python
from tqdm import tqdm,trange
import time
 
for i in trange(100):
  time.sleep(0.1)
  pass
```

## 观察处理的数据

通过`tqdm`提供的`set_description`方法可以实时查看每次处理的数据

```python
from tqdm import tqdm
import time
 
pbar = tqdm(["a","b","c","d"])
for c in pbar:
  time.sleep(1)
  pbar.set_description("Processing %s"%c)
```

## 手动设置处理的进度

通过`update`方法可以控制每次进度条更新的进度

```python
from tqdm import tqdm
import time
 
#total参数设置进度条的总长度
with tqdm(total=100) as pbar:
  for i in range(100):
    time.sleep(0.05)
    #每次更新进度条的长度
    pbar.update(1)
```

## 推荐用法

除了使用`with`之外，还可以使用另外一种方法实现上面的效果

```python
from tqdm import tqdm
import time
 
#total参数设置进度条的总长度
pbar = tqdm(total=100)
for i in range(100):
  time.sleep(0.05)
  #每次更新进度条的长度
  pbar.update(1)
#关闭占用的资源
pbar.close()
```

## 自定义进度条显示信息

通过`set_description`和`set_postfix`方法设置进度条显示信息

```python
from tqdm import trange
from random import random,randint
import time
 
with trange(100) as t:
  for i in t:
    #设置进度条左边显示的信息
    t.set_description("GEN %i"%i)
    #设置进度条右边显示的信息
    t.set_postfix(loss=random(),gen=randint(1,999),str="h",lst=[1,2])
    time.sleep(0.1)
```

## 多层循环进度条

通过`tqdm`也可以很简单的实现嵌套循环进度条的展示

```python
from tqdm import tqdm
import time
 
for i in tqdm(range(20), ascii=True,desc="1st loop"):
  for j in tqdm(range(10), ascii=True,desc="2nd loop"):
    time.sleep(0.01)
```

## 多进程进度条

在使用多进程处理任务的时候，通过tqdm可以实时查看每一个进程任务的处理情况

```python
from time import sleep
from tqdm import trange, tqdm
from multiprocessing import Pool, freeze_support, RLock

L = list(range(9))

def progresser(n):
  interval = 0.001 / (n + 2)
  total = 5000
  text = "#{}, est. {:<04.2}s".format(n, interval * total)
  for i in trange(total, desc=text, position=n,ascii=True):
    sleep(interval)

if __name__ == '__main__':
  freeze_support() # for Windows support
  p = Pool(len(L),
       # again, for Windows support
       initializer=tqdm.set_lock, initargs=(RLock(),))
  p.map(progresser, L)
  print("\n" * (len(L) - 2))
```

## Pandas中使用tqdm

```python
import pandas as pd
import numpy as np
from tqdm import tqdm

df = pd.DataFrame(np.random.randint(0, 100, (100000, 6)))
tqdm.pandas(desc="my bar!")
df.progress_apply(lambda x: x**2)
```

## 注意

在使用`tqdm`显示进度条的时候，如果代码中存在`print`可能会导致输出多行进度条，此时可以将`print`语句改为`tqdm.write`，代码如下

```python
for i in tqdm(range(10),ascii=True):
  tqdm.write("come on")
  time.sleep(0.1)
```


