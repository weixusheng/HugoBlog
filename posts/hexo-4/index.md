# 部署-spawn failed错误的解决方法


## 删除 .deploy_git 文件夹

## 修改配置文件
```
git config --global core.autocrlf false
```

## 依次执行：
```
hexo clean
hexo g
hexo d
```

## 更改用户凭据
控制面板更改用户凭据,github账号密码

