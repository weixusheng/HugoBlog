# Ubuntu护眼软件


20.04竟然没有带护眼模式,我记得18.04有这个模式的...只好自己装一个软件啦
<!-- more-->

## 安装
```bash
sudo apt-get install redshift-gtk
```
## 配置参数
进入~/.config
```bash
touch redshift.conf
```
之后填写配置文件
```bash
sudo vim redshift.conf
```
写入以下内容：
```bash
[redshift]
; 白天屏幕温度
temp-day=5600
; 夜晚屏幕温度
temp-night=4200
; 昼夜是否平滑过度(1/0)
transition=1
; 位置提供方式(redshift -l list)
location-provider=manual

[manual]
; 位置提供方式设置
; 经纬度(北京)
lat=39
lon=116
```
