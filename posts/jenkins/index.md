# Jenkins入门



初识Jenkins
<!--more-->

# 包管理器[Python包管理之poetry的使用](https://www.cnblogs.com/-wenli/p/13337188.html)

# SQLAlchemy之alembic

[SQLAlchemy之alembic_why404-CSDN博客](https://blog.csdn.net/weixin_44733660/article/details/104110236)

# 部署Jenkins服务

[Linux+Jenkins环境搭建以及自动部署django项目_冰棒的博客-CSDN博客_jenkins部署django](https://blog.csdn.net/a877415861/article/details/74544086)

[Jenkins 远程部署Django（Flask）项目_gbfeng123的博客-CSDN博客](https://blog.csdn.net/gbfeng123/article/details/120025234?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-5.pc_relevant_aa&spm=1001.2101.3001.4242.4&utm_relevant_index=8)

[使用Jenkins进行Python项目的持续集成 - 简书 (jianshu.com)](https://www.jianshu.com/p/caa136e191cd)

[使用Jenkins部署Python项目 - 姜小豆 - 博客园 (cnblogs.com)](https://www.cnblogs.com/warcraft/p/9890649.html)

[Jenkins部署git+python项目实现持续集成 - 六三零 - 博客园 (cnblogs.com)](https://www.cnblogs.com/bendouyao/p/9161513.html)

[Jenkins安装与配置(Flask+Gunicorn及React) - 简书 (jianshu.com)](https://www.jianshu.com/p/34e79942697c)

[利用Jenkins + nginx 实现前端项目自动构建与持续集成 - SegmentFault 思否](https://segmentfault.com/a/1190000019212628)

## 反向代理jenkins

[在nginx中设置jenkins的反向代理 - 简书 (jianshu.com)](https://www.jianshu.com/p/8315657465ac)

# fastapi设置根目录映射

[Behind a Proxy - FastAPI (tiangolo.com)](https://fastapi.tiangolo.com/zh/advanced/behind-a-proxy/)

# yum

别执行 yum update

重装yum

```bash
rpm -qa | grep yum # 查看已安装的yum
rpm -aq|grep yum|xargs rpm -e –nodeps # 卸载
cat /etc/redhat-release # 查看当前centos版本
# 下载
wget http://vault.centos.org/7.4.1708/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-42.el7.noarch.rpm
wget http://vault.centos.org/7.4.1708/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget http://vault.centos.org/7.4.1708/os/x86_64/Packages/yum-3.4.3-154.el7.centos.noarch.rpm
# 安装
rpm -ivh yum-*
# 修改源
cd /etc/yum.repos.d
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
yum clean all
yum makecache
```

# Jenkins

## 安装

```bash
yum install java
yum install git
yum install nginx
service nginx start
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key 
yum install jenkins --nogpgcheck
service jenkins restart
```

## 配置文件

```bash
···
    location /jenkins{
        proxy_pass http://jenkins_ip:jenkins_port;
        proxy_redirect http:// https://;
        sendfile off;
        proxy_set_header   Host             $host:$server_port;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_max_temp_file_size 0;
   }
···
```

```bash
vim /etc/sysconfig/jenkins
```

追加

```bash
···
JENKINS_ARGS="--prefix=/jenkins"
···
```
修改配置文件生效

在修改过配置文件后,需要重新设定相应的权限Jenkins才可以正常运行,于是修改`JENKINS_USER="jenkins"`为`root`

```bash
JENKINS_USER="jenkins"
```

重启服务

```bash
nginx -s reload
service jenkins restart
```






