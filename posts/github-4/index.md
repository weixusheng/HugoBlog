# Git 强制拉取覆盖本地



强制拉取覆盖本地<br>
remote Incorrect username or password 报错
<!--more-->

# 强制拉取覆盖本地
```bash
git fetch --all
git reset --hard origin/master
git pull
```

# remote Incorrect username or password 报错
在 vscode 中链接 github(指第一次链接)经常莫名其妙告诉我账号密码错误...可能之前确实输入错了,但是却没法改变
后来查了好久,才知道还能在控制面板里改!
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/github-5.md/1.png)
点击 windows-凭据
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/github-5.md/2.png)
