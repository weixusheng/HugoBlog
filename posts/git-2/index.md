# Git命令-2



Mosh/Git
<!--more-->

```bash
git clone url file_name

# git fetch是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中
# 而git pull 则是将远程主机的最新内容拉下来后直接合并
git pull = git fetch + git merge

git fetch origin master 
# 从远程主机的master分支拉取最新内容 
git merge FETCH_HEAD
# 将拉取下来的最新内容合并到当前所在的分支中

git fetch
git log --online --all --graph
# 此时会出现两个分支
git merge origin/master 

git pull --rebase
# 将remote结点插入到本地最新结点之前
```

## master,origin master, origin/master的区别

- **master** 这个很好理解，它代表本地的某个分支名。
- **origin master** 代表着两个概念，前面的 **origin** 代表远程名，后面的 **master** 代表远程分支名。
- **origin/master** 只代表一个概念，即远程分支名，是从远程拉取代码后在本地建立的一份拷贝（因此也有人把它叫作本地分支）。

举几个例子可能会更加清晰地说明问题：

- 执行 `git fetch origin master` 时，它的意思是从名为 **origin** 的远程上拉取名为 **master** 的分支到本地分支 **origin/master** 中。既然是拉取代码，当然需要同时指定远程名与分支名，所以分开写。
- 执行 `git merge origin/master` 时，它的意思是合并名为 **origin/master** 的分支到当前所在分支。既然是分支的合并，当然就与远程名没有直接的关系，所以没有出现远程名。需要指定的是被合并的分支。
- 执行 `git push origin master` 时，它的意思是推送本地的 **master** 分支到远程 **origin**，涉及到远程以及分支，当然也得分开写了。
- 还可以一次性拉取多个分支的代码：`git fetch origin master stable oldstable`；
- 也还可以一次性合并多个分支的代码：`git merge origin/master hotfix-2275 hotfix-2276 hotfix-2290`；

执行 `git branch -a` 可以查看所有的分支名

```bash
git push -f # 强行推送本地节点,替换远程节点

# 添加标签
git tag v1.0
# 删除远程标签
git push origin --delete v1.0
# 查看branch信息
git branch -vv
# 删除远程库分支
git push -d origin feature/change
# 切换本地分支
git switch master
# 删除本地分支
git branch -d feature/change
```




