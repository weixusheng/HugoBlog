# Ubuntu设定定时任务



Ubuntu crontab
<!--more-->

# 测试脚本

```python
# 测试脚本 timer_test.py
# 路径 /home/li/d/pythonwork/test/timer_test.py 

#coding:utf-8
import time
t = time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(time.time()))
str = '执行时间：' + t + '\n'
print(str)
```

# 设定定时任务

```bash
sudo crontab -e

*/1 * * * * python  /home/path/timer_test.py  >>  /home/path/testcrontab.log 2>&1

# /home/path/testcrontab.log 为日志文件,程序的输出都在该文件中
```

# 查看脚本

```bash
sudo crontab -l     可看到自己编辑好的文件
```

# 时间参数解释

分钟          0 - 59
小时          0 - 23
天            1 - 31
月            1 - 12
星期          0 - 6       0表示星期天

除了这些固定值外，还可以配合星号（*），逗号（,），和斜线（/）来表示一些其他的含义：

星号：表示任意值，比如在小时部分填写 * 代表任意小时（每小时）
逗号：可以允许在一个部分中填写多个值，比如在分钟部分填写 1,3 表示一分钟或三分钟
斜线：一般配合 * 使用，代表每隔多长时间，比如在小时部分填写 */2 代表每隔两分钟。所以 */1 和 * 没有区别

```bash
*   *  *  *  *                  # 每隔一分钟执行一次任务    
0   *  *  *  *                  # 每小时的0分执行一次任务，比如6:00，10:00    
6, 10  *  2  *  *               # 每个月2号，每小时的6分和10分执行一次任务    
*/3, */5  *  *  *  *            # 每隔3分钟或5分钟执行一次任务，比如10:03，10:05，10:06
    
0  7   *  *  *    /bin/ls  #每天早上7点执行一次 /bin/ls 
0  6-12/3  *  12 *  /usr/bin/backup #在 12 月内, 每天的早上 6 点到 12 点中，每隔3个小时执行一次 /usr/bin/backup
0  17  *  *  1-5  mail -s "hi" alex@domain.name < /tmp/maildata #周一到周五每天下午 5:00 寄一封信给 alex@domain.name 
20  0-23/2  *  *  *  echo "haha" >> /tmp/haha.txt #每月每天的午夜 0 点 20 分, 2 点 20 分, 4 点 20 分....向 /tmp/haha.txt 文件中写入 haha
```
