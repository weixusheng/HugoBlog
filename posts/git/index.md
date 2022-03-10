# 推送到Github



讲个笑话,都快2202年了,我还不会推新库...
<!--more-->

# 推送到Github

现在github改成了main...好久没有推新库了,有点懵,今天整理了一下推送代码到github

首先在github上新建repository

```bash
git remote add origin https://url
```

## 查看分支情况

```bash
git branch -a
```

## 创建main分支

创建并切换分支到`main`

```bash
git checkout -b main
```

## 删除本地master分支

```bash
git branch -D master
```

## 将remote的pull下来

```bash
git pull origin main --allow-unrelated-histories 
```

## 推送remote

```bash
git push -u origin main -f
```

# 设置`.git`文件自定义路径

`path`为`.git`文件的路径

```bash
git init --separate-git-dir = path
```

# 设置git推送通过代理端口

查看代理端口 例如 `port: 7890`

```bash
git config --global http.proxy http://127.0.0.1:7890
```

设置代理

```bash
git config --global http.proxy #查看git的http代理配置
git config --global https.proxy #查看git的https代理配置
git config --global -l #查看git的所有配置
```

取消全局代理：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

