# Ubuntu开机挂载Windows磁盘



挂载windows分区挂载到ubuntu的文件管理器中
<!--more-->

&emsp; 由于Ubuntu在开机时,将Windows的磁盘分区当做外部media设备,所以开机的时候并不自动挂载,这样在使用中就有很多不便,例如Ubuntu中的vscode在打开后每次无法自动加载Windows磁盘中的项目

以下为开机自动挂载的设置步骤:

1. 查看分区信息
```
sudo fdisk -l
```
2. 查看磁盘类型
```
sudo blkid
```
例： 得到以下内容
```
Device         Boot     Start       End   Sectors  Size Id Type
/dev/1         2048   1187839   1185792  579M  7 HPFS/NTFS/exFAT
/dev/2      1187840 210903039 209715200  100G  7 HPFS/NTFS/exFAT
/dev/3     210903040 420618239 209715200  100G  7 HPFS/NTFS/exFAT
/dev/4      420620286 500117503  79497218 37.9G  5 Extended
/dev/5      420620288 421595135    974848  476M 83 Linux
/dev/6      421597184 450891775  29294592   14G 83 Linux
/dev/7      450893824 500117503  49223680 23.5G 83 Linux
```
3. 修改配置文件
```
sudo vim /etc/fstab
```
4. 根据2中得到的内容，添加以下内容到/etc/fstab
```
# for Windows 10 C:/
/dev/2 /home/usrname/File  ntfs defaults 0 0
```
注意：其中的/home/usrname/ 路径系统直接创建File文件夹,即C盘

5. 挂载新添加的分区
```
sudo mount -a
```

