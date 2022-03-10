# crond设置定时任务


Linux下用来周期性的执行任务
<!--more-->

crond是Linux下用来周期性的执行某种任务或等待处理某些事件的一个守护进程，与windows下的计划任务类似，可以使用命令：systemctl status crond进行查看。 crond进程定期(每分钟)检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。用户在cron表（也被称为`crontab`文件）指定了定时任务，crontab也就是我们常见的定时任务设置命令。Linux下的任务调度分为两类，系统任务调度和用户任务调度。

因此只需要对`crontab`操作即可

**系统任务调度**：系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。/etc/crontab文件就是系统任务调度的配置文件。

```bash
[root@ ~]# cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# For details see man 4 crontabs
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

**crond是执行任务计划的一个守护进程**

contab才是需要进行配置的

```bash
[root@ ~]# crontab --help
crontab：无效选项 -- -
crontab: usage error: unrecognized option
Usage:
 crontab [options] file
 crontab [options]
 crontab -n [hostname]
Options:
 -u <user>  define user
 -e         edit user's crontab
 -l         list user's crontab
 -r         delete user's crontab
 -i         prompt before deleting
 -n <host>  set host in cluster to run users' crontabs
 -c         get host in cluster to run users' crontabs
 -s         selinux context
 -x <mask>  enable debugging

[root@ ~]# man crontab
NAME
       crontab - maintains crontab files for individual users
SYNOPSIS
       crontab [-u user] file
       crontab [-u user] [-l | -r | -e] [-i] [-s]
       crontab -n [ hostname ]
       crontab -c
DESCRIPTION
       Crontab  is  the  program  used  to install a crontab table file, remove or list the existing tables used to serve the cron(8) daemon.  Each user can have their own crontab, and
       though these are files in /var/spool/, they are not intended to be edited directly.  For SELinux in MLS mode, you can define more crontabs for each range.  For more information,
       see selinux(8).
       In  this  version  of Cron it is possible to use a network-mounted shared /var/spool/cron across a cluster of hosts and specify that only one of the hosts should run the crontab
       jobs in the particular directory at any one time.  You may also use crontab(1) from any of these hosts to edit the same shared set of crontab files, and to set and  query  which
       host should run the crontab jobs.
```

## 安装

```bash
yum install cronie
```

## 启动暂停`crond`

```bash
# 启动守护进程
crond -i

# 关闭守护进程 (没找到别的方法)
ps ax -O uid,uname,gid,group,ppid
kill -9 PID
```

## `crontab`命令

### 编辑定时任务配置文件

```bash
crontab -e
#相当于    vi /var/spool/cron/        定时任务配置文件保存目录
#         vi /var/spool/cron/root    root用户设置的定时任务配置文件
#         vi /var/spool/cron/shuai   shuai用户设置的定时任务配置文件

# 添加任务
*/1 * * * * python  /home/path/timer_test.py  >>  /home/path/testcrontab.log 2>&1
```

### 查看定时任务

```bash
crontab -l (list)
```

### 删除任务

```bash
crontab -r 
```

## 报错解决

报错 `cron: can’t lock /var/run/crond.pid, otherpid may be 2699: Resource temporarily unavailable`

```bash
rm -rf /var/run/crond.pid
```


