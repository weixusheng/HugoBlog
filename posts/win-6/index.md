# Win-OneNote同步



PAC模式下的OneNote同步加速
<!--more-->

# SSR
在酸酸乳 user.rule 中添加如下规则即可。
```bash
! Put user rules line by line in this file. 
! See 
! OneNote Start 
.officeapps.live.com 
.docs.live.net 
! OneNote End```
```

# 其他梯子
打开前端软件，找到 用户自定义pac那里，添加如下规则
这个我暂时还没用上,但道理都是在代理规则中添加指向onenote的网址
这个过程需要抓包软件
```bash
||live.com
||live.net
||contentsync-onenote.com
```

# 抓包软件
Fiddler4

