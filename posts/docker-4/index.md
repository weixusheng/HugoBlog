# VMware安装Centos


VMware安装Centos集群
<!--more-->

设置虚拟机网络模式为`桥接模式`
查看宿主机(当前主机)的网络信息
```bash
ipconfig
```

## 配置虚拟机网络

### 配置网关地址
首先查看`/etc/sysconfig/network-scripts`下的文件
```bash
ls /etc/sysconfig/network-scripts
```
然后编辑`ifcfg-xxx`文件
```bash
sudo vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

```bash
TYPE=Ethernet
BOOTPROTO=static #修改成static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=eno16777736
UUID=bf5337ab-c044-4af7-9143-12da0d493b89
DEVICE=eno16777736
ONBOOT=yes #修改成yes
PEERDNS=yes
PEERROUTES=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPADDR=192.168.1.130 # 自定义虚拟机的ip地址
NETMASK=255.255.255.0 #设置子网掩码
GATEWAY=192.168.1.1  #默认网关
DNS1=192.168.1.1 #DNS
# 与宿主机相同
```

添加网关地址
```bash
sudo vi /etc/sysconfig/network 
```

```bash
NETWORKING=yes
HOSTNAME=xxxx #随便一个名字
GATEWAY=192.168.1.1 #默认网关和宿主机相同
```

### 重启network
```bash
sudo service network restart
```

### 测试网络

```bash
ping 192.168.1.130
ping baidu.com
```

## 安装bash补全功能

由于我安装的是`Minimal`版本的`centos`因此bash没有路径不全功能
```bash
sudo yum -y install bash-completion
```

## 开启SSH服务
### 安装 `openssh-server`
```bash
sudo yum install openssh-server
```

### 配置SSH文件
```bash
sudo vi /etc/ssh/sshd_config
```

```bash
# 去除注释
Port 22 # 开启监听端口
PermitRootLogin yes	# 开启远程登录
PasswordAuthentication yes #开启账号密码登录
```

### 开启服务
```bash
sudo service sshd start
```

### 验证服务开启
```bash
ps -e | grep sshd
```

### 设置SSH开机自启动
```bash
sudo systemctl enable sshd.service
```

```bash
systemctl list-unit-files | grep sshd
```

## 克隆虚拟机

### VMware克隆

![](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Docker/4/img.png)

### 选择`创建完整克隆`

### 更改主机`hostname`

```bash
sudo vi /etc/hostname
```

### 更改`ip`地址

```bash
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

```bash
IPADDR= new_value # 192.168.1.131
```

### 重启网络

```bash
sudo systemctl restart network
```

### 配置hosts

```bash
vi /etc/hosts
```

```bash
192.168.1.130 node-1
192.168.1.131 node-2
```


