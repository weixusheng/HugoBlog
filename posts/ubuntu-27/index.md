# COSCMD工具


可视化界面操作起来很慢(并且linux是Appimage安装包),还是bash来的实在,今天把COSCMD配置了一下
<!--more-->

# 安装pip
```bash
sudo curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3 get-pip.py
```

# 配置文件
```bash
coscmd config -a AChT4ThiXAbpBDEFGhT4ThiXAbp**** -s WE54wreefvds3462refgwewe**** -b examplebucket-1250000000 -r ap-beijing
# -a 密钥 ID hexo 
# -s 密钥 Key 
# -b 指定的存储桶名称
# -r 存储桶所在地域
```

# 上传
**coscmd upload**
上传当前位置 1.png 文件到 存储桶 pat-11 文件夹下(pat-11若没有存在,则新建)
```bash
coscmd upload 1.png pat-11/ #文件
coscmd upload -r <localpath> <cospath> #文件夹
# 忽略 .txt 和 .doc 的后缀文件
coscmd upload -rs /data/examplefolder data/examplefolder --ignore *.txt,*.doc
```

# 下载
```bash
coscmd download <cospath> <localpath> #文件
coscmd download -r <cospath> <localpath> #文件夹
```

# 删除
```bash
coscmd delete <cospath> #文件
coscmd delete -r <cospath> #文件夹
```

# 获取URL
**coscmd signurl**
```bash
coscmd signurl pat-11/1.png
```
