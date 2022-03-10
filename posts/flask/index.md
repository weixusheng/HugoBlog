# Apache部署Flask



Apache部署Flask过程
<!--more-->

# 环境准备工作

##  下载apache v14版本
说明：为了对应mod_wsgi模块的要求，我们选择apacheVC14版本
[下载地址](https://www.apachelounge.com/download/VC14/)
选择版本：httpd-2.4.33-win64-VC14.zip

## 安装vc_redist_x64
在通过httpd -k install -n Apache 命令，把Apache添加到了Windows的本地服务中
当启动这个服务时，提示：1053 错误 - "错误 1053: 服务没有及时响应启动或控制请求"。
表示没有安装:vc_redist_x64 
在apache下载页面可以下载安装

## 下载python3.6
安装flask等项目依赖库
```bash
pip install flask
```

## mod_wsgi模块
### 下载
>说明：参考链接的大神使用的.so模块是需要通过翻墙得到，所以我们需要使用whl文件，mod_wsgi的版本选择是非常有讲究的，它需要对应apache对应的vc编译版本（ap24vc14）与python的对应版本（cp36‑cp36m）以及window的架构（amd64）

[下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mod_wsgi)
选择的版本：mod_wsgi‑4.6.4+ap24vc14‑cp36‑cp36m‑win_amd64.whl

### 安装
```bash
pip install <mod_wsgi文件绝对路径>
```

### 验证安装
```shell
mod_wsgi-express module-config
```
显示文件路径,表示成功
```shell
C:\Users\Administrator>mod_wsgi-express module-config
LoadFile #文件路径
LoadModule wsgi_module #文件路径
WSGIPythonHome #文件路径
```
将整体添加到配置文件**http.conf**模块加载部分

# 修改文件配置
## http.conf
```bash
ServerRoot "c:/Apache24"	#修改成自己的目录地址
Listen 8080					#端口号配置
ServerName localhost:8090   #配置servername
# 服务器请求配置
<Directory "c:/Apache24/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>
# 项目文件配置
<VirtualHost *:8090 >
#ServerAdmin example@xx.com
DocumentRoot D:\Book_Spider
<Directory "D:\Book_Spider">
	Require all granted
    Require host ip
</Directory>
WSGIScriptAlias / D:\Book_Spider\wsgi.wsgi
</VirtualHost>
```

# 启动Apache服务
启动管理员模式的cmd进入到Apache24的bin文件路径下：
## 安装apache作为系统服务：
```bash
httpd -k install
```
## 启动apache：
```bash
httpd -k start
```
## 停止apache服务：
```bash
httpd -k stop
```
## 重启apache服务：
```bash
httpd -k restart
```
