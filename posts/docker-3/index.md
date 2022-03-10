# Docker网络模式


Docker网络模式
<!--more-->

容器之间的通信-根据服务/容器名称通信

# 三种网络模式

安装 Docker 以后，会默认创建三种网络，可以通过 `docker network ls` 查看。

```bash
[root@localhost ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
688d1970f72e        bridge              bridge              local
885da101da7d        host                host                local
f4f1b3cf1b7f        none                null                local
```

| 网络模式  | 简介                                                         |
| --------- | ------------------------------------------------------------ |
| bridge    | 为每一个容器分配、设置 IP 等，并将容器连接到一个 `docker0` 虚拟网桥，默认为该模式。 |
| host      | 容器将不会虚拟出自己的网卡，配置自己的 IP 等，而是使用宿主机的 IP 和端口。 |
| none      | 容器有独立的 Network namespace，但并没有对其进行任何网络设置，如分配 veth pair 和网桥连接，IP 等。 |
| container | 新创建的容器不会创建自己的网卡和配置自己的 IP，而是和一个指定的容器共享 IP、端口范围等。 |

## bridge网路模式

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_1.png">

在该模式中，Docker 守护进程创建了一个虚拟以太网桥 `docker0`，新建的容器会自动桥接到这个接口，附加在其上的任何网卡之间都能自动转发数据包。

　　默认情况下，守护进程会创建一对对等虚拟设备接口 `veth pair`，将其中一个接口设置为容器的 `eth0` 接口（容器的网卡），另一个接口放置在宿主机的命名空间中，以类似 `vethxxx` 这样的名字命名，从而将宿主机上的所有容器都连接到这个内部网络上。

比如运行一个基于 `busybox` 镜像构建的容器 `bbox01`，查看 `ip addr`：

> busybox 被称为嵌入式 Linux 的瑞士军刀，整合了很多小的 unix 下的通用功能到一个小的可执行文件中。

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_2.png">

### 查看网络信息

```bash
ip addr
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_3.png">

​		守护进程会创建一对 **对等虚拟设备接口`veth pair`**，将其中一个接口设置为容器的 `eth0` 接口（容器的网卡），另一个接口放置在宿主机的命名空间中，以类似 `vethxxx` 这样的名字命名。
​		同时，守护进程还会从网桥 `docker0` 的私有地址空间中分配一个 IP 地址和子网给该容器，并设置 docker0 的 IP 地址为容器的默认网关。

### 查看网桥信息

```bash
sudo yum install -y bridge-utils
brctl show
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_4.png">

### 查看每个容器的 IP 和 Gateway

```bash 
docker inspect 容器名称|ID
```

 进行查看，在 `NetworkSettings` 节点中可以看到详细信息。

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_5.png">

### 查看所有 `bridge` 网络模式下的容器

```bash
docker network inspect bridge
```

在 `Containers` 节点中可以看到容器名称

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_6.png">

### container指定bridge模式

关于 `bridge` 网络模式的使用，只需要在创建容器时通过参数 `--net bridge` 或者 `--network bridge` 指定即可(创建容器默认使用的网络模式，这个参数可以省略">

## host 网络模式

- host 网络模式需要在创建容器时通过参数 `--net host` 或者 `--network host` 指定；
- **采用 host 网络模式的 Docker Container，可以直接使用宿主机的 IP 地址与外界进行通信**，若宿主机的 eth0 是一个公有 IP，那么容器也拥有这个公有 IP。同时容器内服务的端口也可以使用宿主机的端口，无需额外进行 NAT 转换；
- host 网络模式可以让容器共享宿主机网络栈，这样的好处是**外部主机与容器直接通信**，但是容器的网络缺少隔离性。

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_7.png">

### 创建host容器

基于 `host` 网络模式创建了一个基于 `busybox` 镜像构建的容器 `bbox02`，查看 `ip addr`

```bash
docker run -it --name bbox2 --net host busybox
/# ip addr
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_8.png">

然后宿主机通过 `ip addr` 查看信息

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_9.png">

### 查看docker网络信息

查看所有 `host` 网络模式下的容器，在 `Containers` 节点中可以看到容器名称

```bash
docker network inspect host
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_10.png">

## none 网络模式

- none 网络模式是指禁用网络功能，只有 lo 接口 local 的简写，代表 127.0.0.1，即 localhost 本地环回接口。在创建容器时通过参数 `--net none` 或者 `--network none` 指定；
- none 网络模式即不为 Docker Container 创建任何的网络环境，容器内部就只能使用 loopback  网络设备，不会再有其他的网络资源。可以说 none 模式为 Docke Container  做了极少的网络设定，但是俗话说得好“少即是多”，在没有网络配置的情况下，作为 Docker  开发者，才能在这基础做其他无限多可能的网络定制开发。这也恰巧体现了 Docker 设计理念的开放。

### 创建none容器

基于 `none` 网络模式创建了一个基于 `busybox` 镜像构建的容器 `bbox03`，查看 `ip addr`

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_11.png">

### 查看docker网络信息

查看所有 `none` 网络模式下的容器，在 `Containers` 节点中可以看到容器名称

```bash
docker network inspect none
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_12.png">

## container 网络模式

- Container 网络模式是 Docker 中一种较为特别的网络的模式。在创建容器时通过参数 `--net container:已运行的容器名称|ID` 或者 `--network container:已运行的容器名称|ID` 指定；

- 处于这个模式下的 Docker 容器会共享一个网络栈，这样两个容器之间可以使用 localhost 高效快速通信

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_13.png">

**Container 网络模式即新创建的容器不会创建自己的网卡，配置自己的 IP，而是和一个指定的容器共享 IP、端口范围等**
同样两个容器除了网络方面相同之外，其他的如文件系统、进程列表等还是隔离的

### 创建container容器

基于容器 `bbox01` 创建了 `container` 网络模式的容器 `bbox04`，查看 `ip addr`

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_14.png">

### 查看docker网络信息

容器 `bbox01` 的 `ip addr` 信息如下

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_15.png">

宿主机的 `ip addr` 信息如下

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_16.png">

Docker 守护进程只创建了一对对等虚拟设备接口用于连接 bbox01 容器和宿主机，而 bbox04 容器则直接使用了 bbox01 容器的网卡信息

这个时候如果将 bbox01 容器停止，会发现 bbox04 容器就只剩下 lo 接口了
当bbox01 容器重启以后，bbox04 容器也重启一下，就又可以获取到网卡信息了

# 自定义网络模式

虽然 Docker 提供的默认网络使用比较简单，但是为了保证各容器中应用的安全性，在实际开发中更推荐使用自定义的网络进行容器管理，以及启用容器名称到 IP 地址的自动 DNS 解析。

> 　　从 Docker 1.10 版本开始，docker daemon 实现了一个内嵌的 DNS server，使容器可以直接通过容器名称进行通信。方法很简单，只要在创建容器时使用 `--name` 为容器命名即可。
>
> 　　但是使用 Docker DNS 有个限制：**只能在 user-defined 网络中使用**。也就是说，默认的 bridge 网络是无法使用 DNS 的，所以我们就需要自定义网络。

## 创建网络

```bash
docker network create network_name#创建自定义网络模式
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_17.png">

创建一个基于 `bridge` 网络模式的自定义网络模式 `custom_network`

```bash
docker network create custom_network # 默认模式为bridge
```

### 查看网络模式

```bash
docker network ls
```

### 创建容器

通过自定义网络模式 `custom_network` 创建容器：

```bash
docker run -di --name bbox05 --net custom_network busybox
```

### 查看容器网络信息

在 `NetworkSettings` 节点中可以看到详细信息

```bash
docker inspect 容器名称|ID
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_18.png">

## 连接新的网络

通过 `docker network connect 网络名称 容器名称` 为容器连接新的网络模式

```bash
docker network connect bridge bbox05
```

通过 `docker inspect 容器名称|ID` 再次查看容器的网络信息，多增加了默认的 `bridge`

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_19.png">

## 断开网络

可以通过 `docker network rm 网络名称` 命令移除自定义网络模式，网络模式移除成功会返回网络模式名称

```bash
docker network rm custom_network
```

> 注意：如果通过某个自定义网络模式创建了容器，则该网络模式无法删除

# 容器间网络通信

接下来我们通过所学的知识实现容器间的网络通信。首先明确一点，容器之间要互相通信，必须要有属于同一个网络的网卡

## 创建容器

先创建两个基于默认的 `bridge` 网络模式的容器

```bash
docker run -di --name default_bbox01 busybox
docker run -di --name default_bbox02 busybox
```

## 查看IP信息

```bash
docker network inspect bridge
```

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_20.png">

## 测试通信-IP

测试两容器间是否可以进行网络通信

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_21.png">

## 通过容器名称

docker daemon 实现了一个内嵌的`DNS server` ，使容器可以直接通过容器名称通信。方法很简单，只要在创建容器时使用 `--name` 为容器命名即可

但是使用 Docker DNS 有个限制：**只能在 user-defined 网络中使用**。也就是说，默认的 bridge 网络是无法使用 DNS 的，所以我们就需要自定义网络

　　我们先基于 `bridge` 网络模式创建自定义网络 `custom_network`，然后创建两个基于自定义网络模式的容器

```bash
docker run -di --name custom_bbox01 --net custom_network busybox
docker run -di --name custom_bbox02 --net custom_network busybox
```

通过 `docker network inspect custom_network` 查看两容器的具体 IP 信息

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_22.png">

## 测试通信-容器名称

<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/3/img_23.png">

两个属于同一个自定义网络的容器是可以进行网络通信的，并且可以使用容器名称进行网络通信

## `bridge`和 `custom_network` 通信

让 `bridge` 网络下的容器连接至新的 `custom_network` 网络即可

```bash
docker network connect custom_network default_bbox01
```


