# 配置云服务器Python环境



远程服务器开发环境
<!--more-->

# Anaconda

## 安装

```bash
bash ~/Downloads/Anaconda3-2020.02-Linux-x86_64.sh # 安装anconda
source ~/.bashrc # 激活
```

## 重装

```bash
sudo su root # 切换root权限
conda info # 查看conda路径
rm -rf /home/ubuntu/anaconda3 # 删除文件
sudo gedit ~/.bashrc # 清除配置文件启动路径

```
在`.bashrc`文件末尾用#号注释掉之前添加的路径(或直接删除), 保存并关闭文件
```bash
#export PATH=/home/lq/anaconda3/bin:$PATH 
source ~/.bashrc # 立即生效
```

# VNC粘贴

```bash
vncconfig& # 这个看起来有用,但是好像不好使
vncconfig -nowin& # 这个有用
```

# 关闭VNC服务

```bash
vncserver -kill :1
# 提示 Killing Xvnc process ID xxx
```

# 查找安装的包

```bash
apt list --installed # 列出所有的包
apt list --installed |grep python # 模糊查找python的包
```

# vscode remote-ssh连接ubuntu

## 账号密码登录

1. 安装`remote-ssh`插件
2. 新建ssh窗口
3. 配置文件选择 `user/xxx/.ssh/config` 文件
4. `config`文件

```bash
Host servername
	HostName ip
	User username
```

## 免密登录

1. window下生成秘钥

   ```bash
   ssh-keygen # 生成id_rsa文件和id_rsa.pub文件
   ```

2. 将`id_rsa.pub`文件拷贝到服务器的`/root/.ssh`目录下

3. 打开`/etc/ssh/sshd_config`文件

   ```bash
   PubkeyAuthentication ys # 去除注释
   AuthorizedKeysFile .ssh/authorized_keys # 去除注释
   ```

4. 执行`cat id_rsa.pub >> authorized_keys`

5. 重启`sshd`服务: 

   ```bash
   sudo /etc/init.d/ssh restart # 重启sshd服务
   /etc/init.d/ssh start # 开启sshd服务
   ```

# vim

```bash
:w # 保存当前文件
:wq # 保存当前文件,并退出
:q! # 放弃修改,退出
:n # 将光标移到第n行
:f # 显示文件名称,总行数
:%s/a/b/g # 将文中所有a替换为b
:split(sp) # 上下分屏显示
:vsplit(vsp) # 左右分屏显示
:set number # 设置显示正文行号
```

# 配置Pycharm远程开发

## 1. 新建项目

file–>settings–>project 或者新建项目时就选好：

<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Typora/image-20211014190659839.png" alt="image-20211014190659839" style="zoom:50%;" />

## 2. 配置ssh环境(省略)

## 3. 配置文件映射关系

<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Typora/image-20211014191355914.png" alt="image-20211014191355914" style="zoom: 80%;" />

<img src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Typora/image-20211014191613236.png" alt="image-20211014191613236" style="zoom:67%;" />

## 注意:要设置登录账号为root, 否则上传项目文件到ubuntu会权限不足

# 服务器设置root登录

```bash
1、先用ubuntu账号登录，执行sudo passwd root

2、按要求输入密码，请牢记。

3、执行sudo vi /etc/ssh/sshd_config

4、找到PermitRootLogin without-password这一行，把后面的without-password改为yes，取消注释，保存文件。

5、执行sudo service ssh restart
```

`PermitRootLogin yes` 允许root登录 设为yes

`PermitRootLogin prohibit-password`  允许root登录，但是禁止root用密码登录

# xlrd报错

**pandas无法打开.xlsx文件，xlrd.biffh.XLRDError: Excel xlsx file； not supported**

原因是最近xlrd更新到了2.0.1版本，只支持.xls文件。所以pandas.read_excel(‘xxx.xlsx’)会报错。

可以安装旧版xlrd1.2.0

```bash
pip uninstall xlrd
pip install xlrd==1.2.0
```
