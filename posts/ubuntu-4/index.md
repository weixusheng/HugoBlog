# Ubuntu关机重启过慢


今天使用Ubuntu遇到了两个问题
1. 重启关机速度过慢
2. 开机后显示WiFi模块工作不正常
最后通过修改系统配置文件和terminal命令解决
<!--more-->

1.重启关机过慢

关机过程中一直显示加载界面，风扇也一直在转，并没有死机，在这种情况下按F2，可以看到正在处理的系统进程，可以看到系统卡在了“A stop job is running for .. ”的等待进程上，由于Ubuntu默认终止进程的等待时间为90s，所以当需要关闭的进程过多时，就会造成一直等待终止进程的假死情况
此时应该强制关机，并且开机后修改配置文件
Terminal执行
```
sudo vim /etc/systemd/system.conf
```
之后修改
```
DefaultTimeoutStartSec=10s
DefaultTimeoutStopSec=10s
```
去掉这两项开头的注释字符，然后修改后面的时间
随后Esc退出编辑模式 ：wq保存更改
Terminal执行
```
systemctl daemon-reload
```
重新加载配置文件

2.开机后WiFi模块工作不正常

执行Terminal命令
```
sudo /etc/init.d/networking restart
```

