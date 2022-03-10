# 部署Docker



Docker 部署 Python 和 Mysql 环境
<!--more-->

# 安装docker

```bash
curl -sSL https://get.daocloud.io/docker | sh
# 启动docker
sudo service docker start
```

# 拉取镜像

```bash
docker pull ubuntu:20.04
docker pull mysql:8.0.26
```

# 操作镜像

## 查看host中的镜像

```bash
docker images
```

## 删除指定id的镜像

```bash
docker rmi <image id>
```

# 操作容器

## 停止容器

```bash
docker stop id
```

## 删除容器

```bash
docker rm id
```

# 运行Image

```bash
docker run -idt --name docker_ubuntu -d ubuntu:20.04
docker run -itd --name docker_mysql -p 3309:3306 -e MYSQL_ROOT_PASSWORD=980215 mysql:8.0.26
# 因为默认的3306端口被占用,所以调整为3309
```

`-p 3309:3306`：-p 宿主机端口:容器端口，即将宿主机3309端口映射到容器的3306端口，在宿主机登录容器数据库的时候，使用宿主机端口，如3309

`MYSQL_ROOT_PASSWORD` 为mysql的默认密码

# 进入ubuntu容器

## 查看容器id

```bash
docker ps -a
```

## 进入容器

```bash
docker exec -it 容器id /bin/bash  

apt update
apt install software-properties-common
add-apt-repository ppa:deadsnakes/ppa
apt install python3.8
ln -s /usr/bin/python3.8 /usr/bin/python # 创建软连接


#win查看端口被占用情况
netstat -an
```

## 进入mysql容器

```bash
#访问mysql
docker exec -it mysql bash
mysql -uroot -p123456 # 进入mysql终端

mysql -uroot -p123456 -h127.0.0.1 -P 3306 -D mysql

# 修改密码
set password for 'username'@'host' = password('newpassword')
```

## 退出容器

```bash
# Ctrl + D 退出容器
```

# MySQL8连接的问题

使用`DBeaver`连接`docker_mysql`会产生报错

`Public Key Retrieval is not allowed`

## 解决方式

报错是安全相关的问题，高版本mysql对安全的管理更加严格，在自己的开发环境下不用那么严格，于是解决思路就是允许其使用Public Key。右键建好的连接，选择编辑连接，找到驱动属性allowPublicKeyRetrieval，将其值设置为TRUE即可

<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Typora/image-20211229112846981.png" alt="image-20211229112846981" style="zoom:50%;" />

# run指令

<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Typora/20191013101404753.png" alt="docker run 命令解释" style="zoom:50%;" />

