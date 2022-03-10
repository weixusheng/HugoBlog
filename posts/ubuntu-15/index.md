# Ubuntu下的Steam中文支持


今天在玩Besiege的时候,好多零件的英文都不认识...在steam更新公告里,游戏是支持中文的,可是设置里中的没有中文选项,只有空白
然后我就开始强迫症折腾了...
首先去网上找汉化教程,安装了MOD模块,以及中文mod包,不过没有用,因为linux的特殊性,就去MOD的github官方库去看看咋回事,发现...早就不支持当前版本了
随后发现创意工坊里有繁体中文包,犹豫了好久装不装,突然想到按照它的安装思路,不如去看看他的包是装在哪里,跟着目录找到了steam语言包的位置,发现有chinese文件啊...
这就有意思了...有文件为什么游戏不显示呢?
莫非是...字体不支持!
网上搜了搜发现果然是这样的!linux下的很多游戏中文都是乱码,因为steam游戏要求的字体没有下载
机智如我
<!-- more-->

**安装以下字体即可**
```bash
sudo apt-get install ttf-wqy-microhei  #文泉驿-微米黑

sudo apt-get install ttf-wqy-zenhei  #文泉驿-正黑

sudo apt-get install xfonts-wqy #文泉驿-点阵宋体
```

