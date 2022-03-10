# Centos部署Fastapi


Centos + gunicorn + Supervisor 部署 Fastapi
<!--more-->

# gunicorn

```bash
# 查看进程树
pstree -ap|grep gunicorn

#重启Gunicorn任务
kill -HUP 14226

#退出Gunicorn任务
kill -9 28097
```

# centos7安装nginx(老版本)

**一. gcc 安装**
安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：

```bash
yum install gcc-c++
```

**二. PCRE pcre-devel 安装**
PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。命令：

```bash
yum install -y pcre pcre-devel
```

**三. zlib 安装**
zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。

```bash
yum install -y zlib zlib-devel
```

**四. OpenSSL 安装**
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。
nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。

```bash
yum install -y openssl openssl-devel
```

**五. nginx安装**

使用`wget`命令下载（推荐）。确保系统已经安装了wget，如果没有安装，执行 yum install wget 安装。

```bash
wget -c https://nginx.org/download/nginx-1.12.0.tar.gz
```

**解压**

依然是直接命令：

```
tar -zxvf nginx-1.12.0.tar.gz
cd nginx-1.12.0
```

**配置**

其实在 nginx-1.12.0 版本中你就不需要去配置相关东西，默认就可以了。当然，如果你要自己配置目录也是可以的。
1.使用默认配置

```
./configure
```

2.自定义配置（不推荐）

```
./configure \
--prefix=/usr/local/nginx \
--conf-path=/usr/local/nginx/conf/nginx.conf \
--pid-path=/usr/local/nginx/conf/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
```

> 注：将临时文件目录指定为/var/temp/nginx，需要在/var下创建temp及nginx目录

**编译安装**

```
make
make install
```

查找安装路径：

```
whereis nginx
```

## **启动、停止nginx**

```
cd /usr/local/nginx/sbin/
./nginx 
./nginx -s stop
./nginx -s quit
./nginx -s reload
```

`./nginx -s quit`:此方式停止步骤是待nginx进程处理任务完毕进行停止。
`./nginx -s stop`:此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

参考: [CentOS7安装Nginx - boonya - 博客园 (cnblogs.com)](https://www.cnblogs.com/boonya/p/7907999.html)

参考2: [centOS7安装nginx及nginx配置_Snow、杨-CSDN博客_centos7安装nginx](https://blog.csdn.net/qq_37345604/article/details/90034424)

## 添加Nginx访问过滤

```bash
vim /usr/local/nginx/conf/nginx.conf

# 在 server内 location的上方添加
allow IP;
allow 127.0.0.1;
deny all;
# 即可使得接口服务只有 IP 和 localhost能访问, 其他IP访问都是403, 保证了服务器的安全性
```

# 端口检查

## fuser

关闭占用80端口的进程

```bash
sudo fuser -k 80/tcp
sudo fuser -k -n tcp 80

# 关闭占用8080端口的进程
sudo fuser -k 8080/tcp
sudo fuser -k -n tcp 8080
sudo fuser -k --namespace tcp 8080
```

-k, --kill kill processes accessing the named file
-n, --namespace 接 命名空间(tcp | udp | file) 接 (端口号 | 文件名)，
如果不会引起歧义的话, 可用：name/space (80/tcp)之类的表示 , 省略 -n。

参考: [CentOS7 关闭占用指定端口的进程_kfepiza的博客-CSDN博客_centos关闭指定端口进程](https://blog.csdn.net/kfepiza/article/details/114959999)

## netstat

1、查看服务器所有被占用端口

```bash
netstat -ant
```

2、验证某个端口号是否被占用

```bash
netstat -tunlp | grep 端口号
```

3、查看所有监听端口号

```bash
netstat -lntp
```

# centos安装python3

## 查看python版本

```bash
[root@localhost ~]# python -V
Python 2.7.5
```

系统默认安装了Python 2.7.5

## 安装依赖

```bash
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```

如果有提示一路选择Y就可以。

安装libffi-devel依赖。

```bash
yum install libffi-devel -y 
```

注意：如果不安装这个包，python3可以装成功，但是后面装flask、uwsgi等依赖python3中有个内置模块叫ctypes时会报错。报错ModuleNotFoundError: No module named ‘_ctypes‘。需要安装依赖包和重新编译安装python3。 参看：https://blog.csdn.net/qq_36416904/article/details/79316972

## 下载稳定版本3.8版

```bash
wget https://www.python.org/ftp/python/3.8.12/Python-3.8.12.tgz
```

## 解压安装python源码包

1、解压

```bash
tar -zxvf Python-3.8.12.tgz
```

2、安装

进入解压后的目录进行编译和安装

```bash
cd Python-3.8.12/
[root@localhost Python-3.8.12]#
[root@localhost Python-3.8.12]# ./configure
[root@localhost Python-3.8.12]# make&&make install
```

## 建立命令软链接

虽然python3.8.12安装成功了，但默认输入python还是显示是2.7版本的。如果要用python3.8.12需要输入python3即可，有时候不太方便。可以通过修改软链接的方式将默认的python指向python3.8.12。
先看一下默认的python及新安装的python3都安装在哪里

```bash
[root@localhost Python-3.8.12]# which python
/usr/bin/python
[root@localhost Python-3.8.12]# which python3
/usr/local/bin/python3
```

可以看到默认的python路径为/usr/bin/python，python3的路径为/usr/local/bin/python3
将python3的软链接加到python上

```bash
[root@localhost Python-3.8.12]# mv /usr/bin/python /usr/bin/python.bak
[root@localhost Python-3.8.12]# ln -s /usr/local/bin/python3 /usr/bin/python
```

通过python -V命令查看python版号，这时python的版本已经是3.8.12了。

```bash
[root@localhost Python-3.8.12]# python -V
Python 3.8.12
```

pip命令也可以修改，python3.8.12默认的pip是pip3，CentOS7的python2.7默认没有安装pip.
输入pip命令的时候提示命令没有找到

```bash
[root@localhost Python-3.8.12]# pip
bash: pip: command not found...
```

这时也可以通过建立软链接的方式将pip命令链接到pip3上。首先看pip3命令在哪?

```bash
[root@localhost Python-3.8.12]# which pip3
/usr/local/bin/pip3
```

然后建立pip到pip3的软链接

```bash
[root@localhost Python-3.8.12]# ln -s /usr/local/bin/pip3 /usr/bin/pip
[root@localhost Python-3.8.12]# pip -V
pip 21.1.1 from /usr/local/lib/python3.8/site-packages/pip (python 3.8)
```

## 配置yum

安装python3改完软链接以后发现yum命令报错了，yum是依赖python2.7的，你把python改成了3.8了，所以报错了。

```bash
[root@localhost Python-3.8.12]# yum
  File "/usr/bin/yum", line 30
    except KeyboardInterrupt, e:
                            ^
SyntaxError: invalid syntax
```

可以修改yum里对python2的依赖即可。虽然安装了python3但是系统里python2依旧还在系统里，可以通过python2来指定用python2.7的命令，首先来看下python2的命令在哪里

```bash
[root@localhost ~]# which python2
/usr/bin/python2
```

可以cd到/usr/bin目录下 通过ls -alh|grep python查看python命令的详细情况。

```bash
[root@localhost bin]# ls -alh|grep python
```

```bash
vi /usr/libexec/urlgrabber-ext-down 
```

修改对python的依赖，修改成python2或python2.7都可以。

```bash
vi /usr/bin/yum
```

修改完这两个文件后，再敲yum命令就不会报错了。

```bash
vi /usr/bin/yum 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2 
vi /usr/libexec/urlgrabber-ext-down 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2
vi /usr/bin/yum-config-manager
#!/usr/bin/python 改为 #!/usr/bin/python2
```

**至此CentOS7环境下python3.8.12已经成功安装！**

参考博客: [CentOS7下安装python3.8 - xiejava - 博客园 (cnblogs.com)](https://www.cnblogs.com/xiejava/p/15541899.html)

# 测试数据库远程连接

```python
from sqlalchemy import create_engine, inspect

engine = create_engine('mysql+pymysql://username:password@ip/db_name', encoding='utf-8', echo=False,
                       pool_size=100, pool_timeout=30)
inspector = inspect(engine)
# 获取全部的schema模式
schemas = inspector.get_schema_names()
print(schemas)
# for schema in schemas:
#     print("schema: %s" % schema)
#     for table_name in inspector.get_table_names(schema=schema):
#         for column in inspector.get_columns(table_name, schema=schema):
#             print("Column: %s" % column)
```

# nginx部署fastapi

在fastapi的main文件中(该步骤请跳过)

```python
if __name__ == '__main__':
    uvicorn.run(app='main:app', host="127.0.0.1", port=8000, reload=True, debug=True)
```

切换到nginx目录配置文件

```bash
cd /usr/local/nginx/conf.d
# 其中nginx.conf为配置文件
vim default.conf
```

## conf编辑文件

```bash
 # 配置转发
 ...
 location / {
	proxy_pass http://127.0.0.1:8000/;
}
...
# 转发文档 接口文档使用默认时，还需添加如下配置
location /openapi.json {
	proxy_pass http://fastapi_host/openapi.json;
}
```

## 安装gunicorn

```bash
pip install gunicorn
```

## 运行gunicorn

```bash
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker # -D 守护启动 -c 配置文件
```

参考博客 [Nginx配置转发FastAPI接口和文档_愤怒的小兵-CSDN博客_fastapi nginx](https://blog.csdn.net/weixin_42078760/article/details/104872328)

## 查看Nginx日志

```bash
cat /usr/local/nginx/logs/error.log
```

## 查看Nginx配置文件

```bash
cat /usr/local/nginx/conf/nginx.conf
```

# 新版nginx部署fastapi

[Nginx+Gunicorn+Supervisor 部署 FastApi 项目 - 年不迈的小清新 - 博客园 (cnblogs.com)](https://www.cnblogs.com/gofun/p/14486216.html)

## Supervisor管理 Gunicorn 进程

### 安装

```bash
yum install supervisor
sudo easy_install supervisor
```

### 配置

```bash
touch supervisor_app.conf
vim supervisor_app.conf

##### 填写如下内容
[include]
files=/etc/supervisord.conf

[program:app_name]
directory= /home/filename/
command= gunicorn main:app -k uvicorn.workers.UvicornWorker -c gunicorn.conf
#####
```

### gunicorn参数解释

```bash
-c CONFIG    : CONFIG,配置文件的路径，通过配置文件启动；生产环境使用；
-b ADDRESS   : ADDRESS，ip加端口，绑定运行的主机；
-w INT, --workers INT：用于处理工作进程的数量，为正整数，默认为1；
-k STRTING, --worker-class STRTING：要使用的工作模式，默认为sync异步，可以下载eventlet和gevent并指定
--threads INT：处理请求的工作线程数，使用指定数量的线程运行每个worker。为正整数，默认为1。
--worker-connections INT：最大客户端并发数量，默认情况下这个值为1000。
--backlog int：未决连接的最大数量，即等待服务的客户的数量。默认2048个，一般不修改；
-p FILE, --pid FILE：设置pid文件的文件名，如果不设置将不会创建pid文件
--access-logfile FILE   ：  要写入的访问日志目录
--access-logformat STRING：要写入的访问日志格式
--error-logfile FILE, --log-file FILE  ：  要写入错误日志的文件目录。
--log-level LEVEL   ：   错误日志输出等级。
--limit-request-line INT   ：  HTTP请求头的行数的最大大小，此参数用于限制HTTP请求行的允许大小，默认情况下，这个值为4094。值是0~8190的数字。
--limit-request-fields INT   ：  限制HTTP请求中请求头字段的数量。此字段用于限制请求头字段的数量以防止DDOS攻击，默认情况下，这个值为100，这个值不能超过32768
--limit-request-field-size INT  ：  限制HTTP请求中请求头的大小，默认情况下这个值为8190字节。值是一个整数或者0，当该值为0时，表示将对请求头大小不做限制
-t INT, --timeout INT：超过这么多秒后工作将被杀掉，并重新启动。一般设定为30秒；
--daemon： 是否以守护进程启动，默认false；
--chdir： 在加载应用程序之前切换目录；
--graceful-timeout INT：默认情况下，这个值为30，在超时(从接收到重启信号开始)之后仍然活着的工作将被强行杀死；一般使用默认；
--keep-alive INT：在keep-alive连接上等待请求的秒数，默认情况下值为2。一般设定在1~5秒之间。
--reload：默认为False。此设置用于开发，每当应用程序发生更改时，都会导致工作重新启动。
--spew：打印服务器执行过的每一条语句，默认False。此选择为原子性的，即要么全部打印，要么全部不打印；
--check-config   ：显示现在的配置，默认值为False，即显示。
-e ENV, --env ENV： 设置环境变量；
```

### 配置文件

```bahs
# gunicorn.conf
# 并行工作进程数
workers = 4
# 指定每个工作者的线程数
threads = 2
# 监听内网端口5000
bind = '127.0.0.1:5000'
# 设置守护进程,将进程交给supervisor管理
daemon = 'false'
# 工作模式协程
# worker_class = 'gevent'
# 设置最大并发量
worker_connections = 2000
# 设置进程文件目录
pidfile = '/var/run/gunicorn.pid'
# 设置访问日志和错误信息日志路径
accesslog = '/var/log/gunicorn_acess.log'
errorlog = '/var/log/gunicorn_error.log'
# 设置日志记录水平
loglevel = 'warning'
```

### 配置文件启动supervisor

```bash
supervisord -c supervisor_app.conf
```

### 启动应用

```bash
sudo supervisorctl start app_name
```

### 常用命令

```bash
supervisorctl start [program_name]
supervisorctl stop [program_name]
supervisorctl restart [program_name] # 重启服务，注意这里不会重新加载配置文件
supervisorctl status # 查看当前程序运行状态
```

重新加载配置文件，重新启动正在运行的服务：

```bash
sudo supervisorctl reload
sudo supervisorctl update
```

### 查看日志

```bash
cat /var/log/supervisor/supervisord.log
```

## 查看程序启动的具体报错

```bash
supervisorctl tail program_name stdout 
```

## 关闭supervisor进程

寻找进程

```bash
ps -ef | grep supervisord
```

杀掉进程

```bash
kill -s SIGTERM 29646
```



# Python-requirement.txt 文件

## 使用freeze

生成 requirements.txt

进入项目根目录，执行以下命令

```bash
pip freeze > requirements.txt
```

使用 requirement.txt 安装第三方库

```bash
pip install -r requirement.txt
```

## 使用pipreqs

```bash
# 安装
pip install pipreqs
# 在当前目录生成
pipreqs . --encoding=utf8 --force
# 安装
pip install -r requirements.txt
```

# 报错解决

## OSError: [Errno 98] Address already in use

指端口被占用，未释放或者程序没有正常结束
查询端口占用进程

```shell
lsof -i:8080 # 查找8080端口进程
```

杀死进程

```shell
kill -9 pidnumber
```

