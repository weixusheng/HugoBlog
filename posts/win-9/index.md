# 跨平台同步书签



Firefox/Chrome利用WebDav同步书签
<!--more-->

之前一直用火狐,今天发现突然访问网站好慢好慢,后来排查发现不是家里网络的问题,是浏览器自身的问题...但是这河狸吗,这不合理呀!edge就一点问题都没有
在设置里逛了好久,发现火狐有一个选项"启用基于Https的DNS",然后关掉...就好了(抓狂...我的大半天没了)
但是后来发现edge也蛮好用的,Chromium插件会更全一点,所以想转到edge呆一阵,然后发现如果同时用火狐和edge的话,书签同步是不互通的,但这能难倒我吗?当然不能!
所以花了好几个小时,终于搞明白啦

# 所需工具
## 浏览器插件
插件 [Floccus](https://floccus.org/)

## webdav
本来想用onedrive,但是查了好久也没查明白怎么调出来
最后只好用了坚果云的webdav,发现同步速度还挺快的!那就这样吧
**创建应用**
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-9/0_.png">
保存好密码

# 配置插件
1. <img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-9/1_.png">

2. <img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/win-9/2_.png">
```shell
https://dav.jianguoyun.com/dav/我的坚果云
```

3. 修改更新时间为5min

4. 注意两个浏览器的同步文件夹部分,因为火狐和edge的书签文件构成是不同的,如果直接同步根目录会发生错误
