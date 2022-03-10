# Ubuntu搭建FTP



Ubuntu20.04搭建FTP服务
<!--more-->

# 搭建FTP服务

## 安装VSFTPD

```bash
apt-get install vsftpd -y
```

## 启动

```bash
netstat -nltp | grep 21
# systemctl start vsftpd.service 手动开启
```

## VSFTPD服务开机启动

```bash
systemctl enable vsftpd.service
```

## 设置访问目录

一般FTP服务都是访问部分目录（比如一个共享文件夹），不可能访问整个系统，所以最好单独建立单独的FTP用户及访问目录。
\- 新建访问目录（/home/ftp）,当然也可以用其他名称，以下以ftp为例

```bash
mkdir /home/ftp
```

## 设置单独的用户访问

```bash
# 新建用户
useradd -d /home/ftp -s /bin/bash ftpuser
# 设置密码
passwd ftpuser
# 删除掉pam.d 中的 vsftpd , 因为该配置文件会导致登录失败：
rm /etc/pam.d/vsftpd
# 限制该用户只能通过FTP 访问
usermod -s /sbin/nologin ftpuser
```

## 配置vsftpd

 vsftpd 配置文件为 `/etc/vsftpd.conf`。首先增加可写权限：`sudo chmod a+w /etc/vsftpd.conf` 。配置包括限制用户对主目录外的访问、 指定允许访问用户的配置文件、使用utf8编码等。可直接将以下内容拷贝到配置文件最下方：

```bash
# 限制用户对主目录以外目录访问
chroot_local_user=YES

# 指定一个 userlist 存放允许访问 ftp 的用户列表
userlist_deny=NO
userlist_enable=YES

# 记录允许访问 ftp 用户列表
userlist_file=/etc/vsftpd.user_list

# 不配置可能导致莫名的530问题
seccomp_sandbox=NO

# 允许文件上传
write_enable=YES

# 使用utf8编码
utf8_filesystem=YES
```

## 新增可访问用户列表

新建文件`/etc/vsftpd.user_list`存放允许访问ftp的用户。上面我们只建立了一个用户：`ftpuser`，所以文件内容应该仅有一行：

```bash
ftpuser
```

若想有多个用户，可以按照以上步骤建立，并加到该文件中，每行一个用户
