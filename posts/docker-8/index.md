# Docker构建前后端服务组


构建前后端服务组
<!--more-->

## 服务器

centos7.9

# Nginx-1.20

## 安装

```bash
yum install nginx
```

## 查看配置文件所在位置

```bash
/usr/sbin/nginx -t
```

## 配置文件

```bash
vim /etc/nginx/nginx.conf
```

```bash
# 访问过滤
allow IP;
allow 127.0.0.1;
deny all;

# 应用映射
location /api {
	proxy_pass http://127.0.0.1:8001/;
}
location /jenkins {
	proxy_pass http://127.0.0.1:8080/jenkins;
	proxy_redirect off;
	proxy_set_header Host $host;
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_set_header X-Forwarded-Proto $scheme;
}
location /DatangQcc {
	proxy_pass http://127.0.0.1:8082/;
}
```

安装依赖库

```bash
# -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install fastapi
apschedule
uvicorn
pymysql
```

error_log

```bash
cat /var/log/nginx/error.log;
```

# docker进程管理

```bash
yum install procps #安装ps
```

查看进程

```bash
ps ax -O uid,uname,gid,group,ppid
```

杀死进程

```bash
kill -9 PID
```



重启docker

```bash
systemctl restart docker
```

# docker安装

## 修改镜像源

```bash
# 编辑配置文件
vi /etc/docker/daemon.json

{
	"registry-mirrors": ["http://hub-mirror.c.163.com"]
}

# 重启docker服务
systemctl restart docker.service
```

## 拉取镜像

```bash
docker pull mysql:8.0.26
docker pull nginx:1.20.1
docker pull rockylinux:8.5
```



## 数据库mysql

docker_mysql

```bash
docker run -itd --name docker_mysql -p 3309:3306 -e MYSQL_ROOT_PASSWORD=980215 mysql:8.0.26
```



## 后端

docker_fastapi

```bash
docker run -itd --name docker_fastapi -p 8081:80 -v /home/DockerVolume/fastapi/data:/home python:3.9
```

因为没有系统层面的控制,但是想用到supervisor,因此还是装系统容器吧

发现`centos8`被官方抛弃了,以后也不会维护了,转向了`rockylinux`

安装

```bash
docker pull rockylinux:8.5
docker run -itd --name docker_rocky -p 8001:80 -p 8002:8080 -v /home/DockerVolume/rocky:/home rockylinux:8.5
```

docker_rockylinux

```bash
# 首先安装EPEL源
yum install epel-release
# 安装supervisor
yum install supervisor

# 安装python
yum install python39
# 链接python3.9到python
# 链接pip3.9到pip
ln -s /usr/bin/python3.9 /usr/bin/python
ln -s /usr/bin/pip3.9 /usr/bin/pip

# (不需要)
# 更换yum源
# 设置镜像变量
MIRROR=mirrors.aliyun.com/rockylinux
# 执行替换
sudo sed -i.bak \
-e "s|^mirrorlist=|#mirrorlist=|" \
-e "s|^#baseurl=|baseurl=|" \
-e "s|dl.rockylinux.org/\$contentdir|$MIRROR|" \
/etc/yum.repos.d/Rocky-*.repo

sudo dnf makecache

# (不需要)
# 设置全局变量
vim /etc/profile
# 在最后添加
export PATH=/usr/local/bin:$PATH
# 刷新配置文件
source /etc/profile
```

# 项目代码修改

```bash
# fastapi
0.0.0.0:port
```



## 前端

docker_nginx

```bash
docker run -itd --name docker_nginx -p 8082:80 -v /home/DockerVolume/nginx/data:/home nginx:1.20
```

