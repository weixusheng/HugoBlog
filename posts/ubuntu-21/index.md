# Ubuntu挂载Onedrive


昨天折腾了一晚上,过程很是艰难,不过还好最后配置成功了,将onedrive挂载上了
使用到一个开源的软件"rclone",英文文档好难读...而且感觉写的还很复杂,最后在一些大佬写的blog帮助下终于配置成功
<!-- more-->

[rclone官网](https://rclone.org/)
```
版本号:
Ubuntu20.04
rclone v1.51.0
onedrive 个人版
```
# 命令行安装
[官方安装文档](https://rclone.org/downloads/)
```bash
sudo curl https://rclone.org/install.sh | sudo bash
```
也可以下载deb安装包

# 配置
## 获得onedrive授权
```bash
weixusheng@weixusheng-ThinkPad-S5:~$ sudo rclone authorize "onedrive"
2020/05/14 19:55:04 NOTICE: Config file "/home/weixusheng/.config/rclone/rclone.conf" not found - using defaults
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=1PaeHX9ECA6DpqWDET_6Gw
Log in and authorize rclone for access
Waiting for code...
Got code
Paste the following into your remote machine --->
{"access_token":"*****"}        #得到授权码
<---End paste
```
## 填写配置文件
### 新建
```bash
weixusheng@weixusheng-ThinkPad-S5:~$ sudo rclone config
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
```

### 命名
```bash
name> one-drive
```

### 选择类型
```bash
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / 1Fichier
   \ "fichier"
 2 / Alias for an existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
.....省略.....
23 / Microsoft OneDrive
   \ "onedrive"
Storage> 23
** See help for onedrive backend at: https://rclone.org/onedrive/ **
```
### 填写client_id和client_secret
直接enter跳过
```bash
Microsoft App Client Id
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_id> 
Microsoft App Client Secret
Leave blank normally.
Enter a string value. Press Enter for the default ("").
client_secret> 
```

### advanced config及auto config
选择no
```bash
Edit advanced config? (y/n)
y) Yes
n) No (default)
y/n> n
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes (default)
n) No
y/n> n
```

### 填写token
```bash
For this to work, you will need rclone available on a machine that has a web browser available.
Execute the following on your machine (same rclone version recommended) :
	rclone authorize "onedrive"
Then paste the result below:
result> {"access_token":"*****"}
```

### 选择onedrive中的服务
```bash
Choose a number from below, or type in an existing value
 1 / OneDrive Personal or Business
   \ "onedrive"
 2 / Root Sharepoint site
   \ "sharepoint"
 3 / Type in driveID
   \ "driveid"
 4 / Type in SiteID
   \ "siteid"
 5 / Search a Sharepoint site
   \ "search"
Your choice> 1
Found 1 drives, please select the one you want to use:
0:  (personal) id=22994754919d54cc
Chose drive to use:> 0
Found drive 'root' of type 'personal', URL: https://onedrive.live.com/?cid=22994754919d54cc
Is that okay?
y) Yes (default)
n) No
y/n> y
```

### 成功生成配置文件
```bash
--------------------
[tronwei]
type = onedrive
token = {"*****"}
drive_id = 22994754919d54cc
drive_type = personal
--------------------
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y
```

完成配置

# 挂载
## 新建文件夹(挂载的目标文件夹)
```bash
#新建本地文件夹，路径自己定，即下面的LocalFolder
sudo mkdir /root/OneDrive
```

## 挂载为磁盘
```bash
#DriveName、Folder、LocalFolder参数根据说明自行替换
sudo rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
```
 - DriveName为初始化配置填的name
 - Folder为OneDrive里的文件夹(可以为根目录'/')
 - LocalFolder为本地文件夹。

举例
```bash
sudo rclone mount one-drive:/ /home/onedrive --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
```

挂载成功后，输入df -h命令查看即可！
```bash
weixusheng@weixusheng-ThinkPad-S5:~$ df -h
文件系统        容量  已用  可用 已用% 挂载点
/dev/sdb8       123G   34G   83G   30% /
one-drive:      1.1T  113G  927G   11% /home/onedrive
```

# systemctl开机自启rclone挂载
## 安装systemctl
```bash
sudo apt install systemd-sysv && reboot
```

## 新建service文件
1. 进入systemd目录(/etc/systemd/system)，创建文件rclone.service
```bash
sudo gedit rclone.service
```
2. 填写以下
```
[Unit]
Description=rclone
[Service]
User=root
ExecStart=sudo rclone mount one-drive:/ /home/onedrive --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
Restart=on-abort
[Install]
WantedBy=multi-user.target
```
3. 保存退出

## systemctl命令  
1. 开机自启：
```bash
systemctl enable rclone
```

2. 启动：
```bash
systemctl start rclone
```

3. 重启：
```bash
systemctl restart rclone
```

4. 停止：
```bash
systemctl stop rclone
```

5. 状态：
```bash
systemctl status rclone
```

# 上传与下载
将文件复制到网盘中为上传
从网盘复制文件到本地为下载


至此重启电脑就会发现,网盘已挂载
rclone还有很多命令,由于现在是图形界面环境,所以先解决了能不能用的问题
命令慢慢学,以后再更新

[参考blog-1](https://www.moerats.com/archives/491/comment-page-2)
[参考blog-2](https://blog.csdn.net/yichurou2981/article/details/103548190)
