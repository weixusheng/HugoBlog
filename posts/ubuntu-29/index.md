# GRUB2 引导顺序


首先查看开机时,在GRUB中,windows处在第几项,如果当前windows在第四项,则此时需要改为3,因为GRUB从第0项开始计数
```bash
$ sudo gedit /etc/default/grub
```
找到set_default 改为 3
保存,重启电脑,即可发现 默认光标移至第四项
