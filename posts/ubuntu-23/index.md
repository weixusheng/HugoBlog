# Ubuntu20.04软件管理命令



软件管理命令汇总

<!-- more-->

# 软件更新
升级安装包相关的命令,刷新可安装的软件列表(但是不做任何实际的安装动作)
```bash
sudo apt-get update
```
进行安装包的更新(软件版本的升级)
```bash
sudo apt-get upgrade
```
dist-upgrade会比upgrade更智能地处理需要更新的软件包的依赖关系(推荐)
```bash
sudo apt-get dist-upgrade
```
进行系统版本的升级(Ubuntu版本的升级)，Ubuntu官方推荐的系统升级方式,若加参数-d还可以升级到开发版本,但会不稳定
```bash
sudo do-release-upgrade
```
清理旧版本的软件缓存
```bash
sudo apt-get autoclean
```
清理所有软件缓存
```bash
sudo apt-get clean 
```
删除系统不再使用的孤立软件
```bash
sudo apt-get autoremove
```

# 删除软件
如果知道要删除软件的具体名称可以使用
```bash
sudo apt-get remove --purge 软件名称  
sudo apt-get autoremove --purge 软件名称
```
如果不知道要删除软件的具体名称可以使用
```bash
dpkg --get-selections | grep ‘软件相关名称’
sudo apt-get purge 一个带core的package，如果没有带core的package，则是情况而定。
```


