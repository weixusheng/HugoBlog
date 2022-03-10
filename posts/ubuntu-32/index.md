# 云服务器部署hexo



Nginx 部署 hexo
<!--more-->

# 安装环境

## 安装Nginx和Git

```bash
apt-get update
apt-get install git nginx -y
```

## 建立文件

```bash
mkdir /var/repo/
# 修改权限
chown -R $USER:$USER /var/repo/
chmod -R 755 /var/repo/
```

## 创建远程仓库

```bash
cd /var/repo
git init --bare CloudHexo.git
```

# 配置Nginx文件目录

```bash
mkdir -p /var/www/hexo

chown -R $USER:$USER /var/www/hexo
chmod -R 755 /var/www/hexo
```

修改 Nginx 的 `default` 文件使得 root 指向刚刚创建的 `/var/www/hexo`目录：

```bash
vim /etc/nginx/sites-available/default
```
替换为以下内容
```bash
server {
   listen 443 ssl;
    #填写绑定证书的域名
    server_name tronwei.com;
    #证书文件名称
    ssl_certificate  cert/tronwei.com_bundle.crt;
    #私钥文件名称
    ssl_certificate_key cert/tronwei.com.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
            #网站主页路径。此路径仅供参考，具体请您按照实际目录操作。
            #例如，您的网站运行目录在/etc/www下，则填写/etc/www。
        root /var/www/hexo;
        index index.html index.htm;
    }
}
server {
    listen 80;
    #填写绑定证书的域名
    server_name tronwei.com;
    #把http的域名请求转成https
    return 301 https://$host$request_uri;
}
```

[腾讯云部署证书](https://cloud.tencent.com/document/product/400/35244)

## 重启 nginx 服务

```bash
service nginx restart
```

# 创建Git钩子(hooks)

执行下面的命令，在自动生成的`ganahBlog.git/hooks` 目录下创建一个新的钩子文件：

```bash
vim /var/repo/CloudHexo.git/hooks/post-receive
```

打开文件后，加入下面的代码：

```bash
#!/bin/bash
git --work-tree=/var/www/hexo --git-dir=/var/repo/CloudHexo.git checkout -f
```

将文件保存（方法参加上文）后，赋予该文件可执行权限：

```bash
chmod +x /var/repo/CloudHexo.git/hooks/post-receive
```

# 使用 Git 部署本地 Hexo 到远端服务器

服务器地址添加到受信任的站点，在本地任意目录从服务器上把`hexo_static`仓库克隆下来：

```text
git clone root@{云服务器IP}:/var/repo/CloudHexo.git
```

编辑站点配置文件`_config.yml` , 将 url 改成`https://{云服务器IP}/`

将 deploy 目标改为 `{服务器用户名}@{服务IP}:/var/repo/CloudHexo.git`：

```bash
hexo clean && hexo g -d # 部署
```






