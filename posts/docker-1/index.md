# Docker基本概念



Docker常用命令
<!--more-->

# Docker的组成

![img_1](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/1/img_1.png)

`Docker` 在运行时分为 `Docker 引擎（服务端守护进程）` 和 `客户端工具`，我们日常使用各种 `docker 命令`，其实就是在使用 `客户端工具` 与 `Docker 引擎` 进行交互。

## Docker客户端

Docker 是一个客户端-服务器（C/S）架构程序。Docker 客户端只需要向 Docker  服务器或者守护进程发出请求，服务器或者守护进程将完成所有工作并返回结果。Docker 提供了一个命令行工具 Docker 以及一整套  RESTful API。你可以在同一台宿主机上运行 Docker 守护进程和客户端，也可以从本地的 Docker  客户端连接到运行在另一台宿主机上的远程 Docker 守护进程。

## Host主机(Docker引擎)

一个物理或者虚拟的机器用于执行 Docker 守护进程和容器

## Image镜像

什么是 Docker 镜像？简单的理解，Docker 镜像就是一个 Linux 的文件系统（Root FileSystem），这个文件系统里面包含可以运行在 Linux 内核的程序以及相应的数据。

通过镜像启动一个容器，一个镜像就是一个可执行的包，其中包括运行应用程序所需要的所有内容：包含代码，运行时间，库，环境变量和配置文件等。

Docker 把 App 文件打包成为一个镜像，并且采用类似多次快照的存储技术，可以实现：

- 多个 App 可以共用相同的底层镜像（初始的操作系统镜像）；
- App 运行时的 IO 操作和镜像文件隔离；
- 通过挂载包含不同配置/数据文件的目录或者卷（Volume），单个 App 镜像可以用来运行无数个不同业务的容器。

## Container容器

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

| Docker | 面向对象 |
| :----- | :------- |
| 容器   | 对象     |
| 镜像   | 类       |

## Volume数据卷

实际上我们的容器就好像是一个简易版的操作系统，只不过系统中只安装了我们的程序运行所需要的环境，前边说到我们的容器是可以删除的，那如果删除了，容器中的程序产生的需要持久化的数据怎么办呢？容器运行的时候我们可以进容器去查看，容器一旦删除就什么都没有了。

所以数据卷就是来解决这个问题的，是用来**将数据持久化到我们宿主机**上，与**容器间实现数据共享**，简单的说就是将宿主机的目录映射到容器中的目录，应用程序在容器中的目录读写数据会同步到宿主机上，这样容器产生的数据就可以持久化了，比如我们的数据库容器，就可以**把数据存储到我们宿主机上的真实磁盘中**。

## Registry镜像存储中心

可以将镜像上传到**Docker Hub**作为公共镜像
也可以在本地搭建局域网**Docker Hub**,搭建权限分级

# Centos安装Docker引擎

```bash
# 安装 yum-utils
sudo yum install -y yum-utils
# 设置 yum 源为阿里云方便下载 Docker Engine
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

## Docker常用命令

```bash
# 启动 docker
sudo systemctl start docker
# 停止 docker
sudo systemctl stop docker
# 重启 docker
sudo systemctl restart docker
# 设置开机启动
sudo systemctl enable docker
# 查看 docker 状态
sudo systemctl status docker
# 查看 docker 内容器的运行状态
sudo docker stats
# 查看 docker 概要信息
sudo docker info
# 查看 docker 帮助文档
sudo docker --help
```

## Docker版本查看

```bash
docker -v
docker version
```

## 配置镜像源

```bash
vi /etc/docker/daemon.json # 编辑配置文件
```

在文件中输入以下内容

```bash
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn", "http://hub-mirror.c.163.com"]
}
```

重新加载配置

```bash
# 重新加载某个服务的配置文件
sudo systemctl daemon-reload
# 重新启动 docker
sudo systemctl restart docker
```

# 镜像命令

## 查看镜像

```bash
docker images
```

| 名称 |     作用     |
| --- | ---------- |
|`REPOSITORY`|镜像在仓库中的名称，本文中以后都简称镜像名称|
| `TAG`|镜像标签|
|`IMAGE ID`|镜像 ID|
|`CREATED`|镜像的创建日期（不是获取该镜像的日期）|
|`SIZE`|镜像大小|
>这些镜像都是存储在 Docker 宿主机的 `/var/lib/docker` 目录下。

## 搜索镜像

```bash
docker search image_name
```

| 名称                                                         | 作用 |
| ------------------------------------------------------------ | ---- |
|`NAME`|镜像名称|
|`DESCRIPTION`|镜像描述|
|`STARS`|用户评价，反映一个镜像的受欢迎程度|
|`OFFICIAL`|是否为官方构建|
|`AUTOMATED`|自动构建，表示该镜像由 Docker Hub 自动构建流程创建的|

## 拉取镜像

```bash
docker pull docker_name
```

## 删除镜像

```bash
# 按照ID删除
docker rmi docker_id
# 删除多个镜像
docker rmi docker_id1 docker_id2 docker_id3
# 查询所有镜像并删除
docker rmi 'docker images -q'
```

# 容器命令

## 查看容器

```bash
docker ps
```

| 名称 | 作用 |
| ---- | ---- |
|CONTAINER ID|容器 ID|
|IMAGE|所属镜像|
|COMMAND||
|CREATED|创建时间|
|STATUS|容器状态|
|PORTS|端口|
|NAMES|容器名称|

## 查看停止的容器

```bash
docker ps if status=exited
```

## 查看所有容器

```bash
docker ps -a
```

## 查看最后一次运行的容器

```bash
docker ps -l
```

## 列出最近创建的 n 个容器

```bash
docker ps -n 5
```

## 创建与启动容器

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

| 名称 | 作用 |
| ---- | ---- |
|-i|表示运行容器|
|-t|表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端|
|--name|为创建的容器命名|
|-v|表示目录映射关系（前者是宿主机目录，后者是映射到宿主机上的目录），可以使用多个 -v 做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上|
|-d|在 run 后面加上 -d 参数，则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加 -i -t 两个参数，创建容器后就会自动进容器里）|
|-p|表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个 -p 做多个端口映射|
|-P|随机使用宿主机的可用端口与容器内暴露的端口映射|

## 创建并进入容器

通过镜像 AA 创建一个容器 BB，运行容器并进入容器的 `/bin/bash`

```bash
docker run -it --name 容器名称 镜像名称:标签 /bin/bash
```

> 注意：Docker 容器运行必须有一个前台进程， 如果没有前台进程执行，容器认为是空闲状态，就会自动退出

## 退出当前容器

```bash
exit
```

## 守护式方式创建容器

```bash
docker run -di --name 容器名称 镜像名称:标签
```

## 登录守护式容器方式

```bash
docker exec -it 容器名称|容器ID /bin/bash
```

## 停止与启动容器

```bash
# 停止容器
docker stop 容器名称|容器ID
# 启动容器
docker start 容器名称|容器ID
```

## 文件拷贝

```bash
docker cp 容器名称:容器目录 需要拷贝的文件或目录
```

# 目录挂载(容器数据卷操作)

我们可以在创建容器的时候，将宿主机的目录与容器内的目录进行映射，这样我们就可以通过修改宿主机某个目录的文件从而去影响容器，而且这个操作是双向绑定的，也就是说容器内的操作也会影响到宿主机，实现备份功能。

但是容器被删除的时候，宿主机的内容并不会被删除。如果多个容器挂载同一个目录，其中一个容器被删除，其他容器的内容也不会受到影响。

> 容器与宿主机之间的数据卷属于引用的关系，数据卷是从外界挂载到容器内部中的，所以可以脱离容器的生命周期而独立存在，正是由于数据卷的生命周期并不等同于容器的生命周期，在容器退出或者删除以后，数据卷仍然不会受到影响，数据卷的生命周期会一直持续到没有容器使用它为止。

创建容器添加 `-v` 参数，格式为`宿主机目录:容器目录`

```bash
docker run -di -v /mydata/docker_centos/data:/usr/local/data --name centos7-01 centos:7
# 多目录挂载
docker run -di -v /宿主机目录:/容器目录 -v /宿主机目录2:/容器目录2 镜像名
```

> 目录挂载操作可能会出现权限不足的提示。这是因为 CentOS7 中的安全模块 SELinux 把权限禁掉了，在 docker run 时通过 `--privileged=true` 给该容器加权限来解决挂载的目录没有权限的问题。

## 匿名挂载

匿名挂载只需要写容器目录即可，容器外对应的目录会在 `/var/lib/docker/volume` 中生成

```bash
# 匿名挂载
docker run -di -v /usr/local/data --name centos7-02 centos:7
# 查看 volume 数据卷信息
docker volume ls
```

## 具名挂载

具名挂载就是给数据卷起了个名字，容器外对应的目录会在 `/var/lib/docker/volume` 中生成

```bash
# 匿名挂载
docker run -di -v docker_centos_data:/usr/local/data --name centos7-03 centos:7
# 查看 volume 数据卷信息
docker volume ls
```

## 指定目录挂载

一开始给大家讲解的方式就属于指定目录挂载，这种方式的挂载不会在 `/var/lib/docker/volume` 目录生成内容

```bash
docker run -di -v /mydata/docker_centos/data:/usr/local/data --name centos7-01 centos:7
# 多目录挂载
docker run -di -v /宿主机目录:/容器目录 -v /宿主机目录2:/容器目录2 镜像名
```

## 查看目录挂载关系

通过 `docker volume inspect 数据卷名称` 可以查看该数据卷对应宿主机的目录地址

```bash
[root@localhost ~]# docker volume inspect docker_centos_data
[
    {
        "CreatedAt": "2020-08-13T20:19:51+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/docker_centos_data/_data",
        "Name": "docker_centos_data",
        "Options": null,
        "Scope": "local"
    }
]
```

通过 `docker inspect 容器ID或名称` ，在返回的 JSON 节点中找到 `Mounts`，可以查看详细的数据挂载信息

## 设置只读/只写

```bash
# 只读。只能通过修改宿主机内容实现对容器的数据管理。
docker run -it -v /宿主机目录:/容器目录:ro 镜像名
# 读写，默认。宿主机和容器可以双向操作数据。
docker run -it -v /宿主机目录:/容器目录:rw 镜像名
```

## volumes-from（继承）

```bash
# 容器 centos7-01 指定目录挂载
docker run -di -v /mydata/docker_centos/data:/usr/local/data --name centos7-01 centos:7
# 容器 centos7-04 和 centos7-05 相当于继承 centos7-01 容器的挂载目录
docker run -di --volumes-from centos7-01:ro --name centos7-04 centos:7
docker run -di --volumes-from centos7-01:rw --name centos7-05 centos:7
```

## 查看容器详细信息

```bash
docker inspect 容器名称|容器ID
```

## 删除容器

```bash
# 删除指定容器
docker rm 容器名称|容器ID
# 删除多个容器
docker rm 容器名称|容器ID 容器名称|容器ID
```


