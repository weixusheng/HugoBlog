# Git命令



Mosh/Git
<!--more-->

# 初始化git
```bash
git config --global user.name "name"
git config --global user.email tronwei@163.com
```

## set default editor
```bash
git config --global core.editor "code --wait"
```

## edit config
```bash
git config --global -e
```

## 设置结尾符号
兼容windows和linux文件
```bash
git config --global core.autocrlf true # windows
git config --global core.autocrlf input # macos/linux
```

## get help
```bash
git config --help
git config --h # get the short help
```

# 创建项目
## take the snapshot
```bash
mkdir moon_file
cd moob_file
git init
ls -a # 显示所有文件(隐藏文件)
```

## 写入文件
```bash
echo hello >> file1.txt
git status
git add file1.txt
```

## add+commit
```bash
git commit -am "initial commit"
```

## 查看staging area
```bash
git ls-files
```

## rename file
```bash
mv file1.txt main.js
git add file1.txt # track the deleted file1.txt
git add main.js # track the new file main.js

# 简单的方式,直接用git命令rename
git mv file1.txt main.js
```

## echo > 和 >>
```bash
echo "xxx" >> file.txt # 重写文件为"xxx"
echo "xxx" > file.txt # 追加到文件最后"xxx"
```

## 添加gitignore文件
```bash
echo logs/ > .gitigore # logs文件夹下的文件都忽略
```

## 删除staging area的文件
```bash
git rm -h # 查看help
git rm --cached -r bin/ # -r 参数 recursive 递归删除目录
```

# commit
## 可视化查看更改
```bash
git config --global diff.tool vscode
git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE"
git config --global -e
# 在最后的参数中添加 "$LOCAL $REMOTE"
git difftool # 查看当前文件的更改
git difftool --staged # 查看stage中与上次commit的对比
```

## 查看具体commit内容
```bash
git show id # id 为commmit的id
git show id:.gitignore # 查看具体文件.gitignore
```

## 撤回更改
```bash
git restore --staged . # 撤回到上一次的commit
```

## 还原删除文件
```bash
git rm file1.txt
git commit -m "delete the file"
git restore --sourse=HEAD~1 file1.txt
```

# git log
## 查看历史
```bash
git log
git log --oneline # 简洁模式
git log --oneline --reverse # 按照时间轴(反向)顺序显示
```

## 筛选参数 
```bash
git log --stat
git log --oneline -3 #last three command
git log --oneline --author="name"   # 根据提交作者查询
git log --oneline --before="2021-4-4"   # 根据时间查询
git log --oneline --after="2021-4-1"
git log --oneline --after="yesterday"
git log --oneline --after="one week ago"
git log --oneline --grep="search_thing" # grep: 正则表达式
git log --oneline -S"hello world" # 查看"hello world"什么时候添加到代码中的'
git log --oneline toc.txt # 查看toc.txt文件被修改过的记录
git log --oneline --patch toc.txt # 查看补丁详情
git show HEAD~2 --name-only # 查看更改的文件
git show HEAD~2 --name-status # 查看被修改文件的状态

git log --oneline --all # 查看所有结点
git checkout master # 将head转移到master
```

## 查看contributor的commit
```bash
git shortlog #查看完整信息
git shortlog -n -s -e # -n查看提交次数 -s省略详细信息 -e显示email
```

## 查看单个文件的修改记录
```bash
git log --oneline --stat toc.txt # stat显示修改状态
```

# git bisect
## 查找bug
bisect(二分法查询bug)
一端标记good, 一端标记bad,之后HEAD指向中间结点
```bash
git bisect start
git bisect bad
git bisect good commit_id #此时HEAD指向中间结点
git log --oneline --all # 查看所有结点
```

# checkout
恢复删除的文件
```bash
git checkout commit_id toc.txt # checkout检出(撤销)
```

# tag
```bash
git tag v1.0 commit_id # 增加标签
git tag -n # 查看标签历史 
git tag -d v1.1 # 删除标签
```

# branch
## 分支操作
```bash
git branch bugfix #创建分支
git branch #显示所有分支
git switch bugfix #切换分支
git branch -m bugfix bugfix/signup-bug #重命名
git branch -d bugfix # 删除分支
git branch -D bugfix # 强制删除分支
git log master..bugfix # 查看分支之间的区别(commit)
git diff master..bugfix #查看分支之间的详细区别
git diff --name-only master..bugfix #查看分支之间的区别(受影响的文件名)
git diff --name-stat master..bugfix #查看分支之间的区别(文件状态)
```

## stash缓存区
```bash
git stash push -m "new stash push" # 提交缓存
git stash -a -m "push all stash" # 提交所有文件进入缓存
git stash list # 查看所有缓存
git stash drop 1 # 删除缓存
git stash clear # 清空缓存
git stash pop # 恢复缓存
git stash apply # 恢复缓存
```

## Merge
### fast-forward merges
fast-forward是直接将HEAD指向分支
```bash
git merge bugfix # 合并分支到master
git switch -C bugfix # 创建分支并切换到分支

```
### no-fase-forward merges
将master和分支,作为一个新的commit进行合并
```bash
git merge --no-ff bugfix # 合并分支(no fast forward)
git config ff no # 关闭fast forward
git config --global ff no
```

## 查看分支图像
```bash
git log --oneline --all --graph
```

## 查看分支
```bash
git branch --merged # 查看已并入master的分支
git branch --no-merged # 查看没有并入master的分支
```

## 分支冲突
如果在合并时,存在文件被分支和master同时修改,那么就会发生冲突
手动解决方式: 在vscode中,将分支修改或master修改删除,再commit

## 可视化合并分支
使用工具`p4merge`
```bash
git config --global merge.tool p4merge # 设置p4merge为默认工具
git config --global mergetool.p4merge.path "D:/p4merge/" # 设定p4merge程序路径
git mergetool
```

## 终止merge
```bash
git merge --abort
```

## 撤销merge
**git reset** –-soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可；
**git reset** -–hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容，撤销的commit中所包含的更改被冲掉；
```bash
git reset --hard HEAD~1 # 撤回到上一个master结点
```

## squash merging
将分支节点进行合并,合并出的新节点,作为新的master结点
此时新节点只有一个parent--->master
```bash
git merge --squash bugfix
```

# rebasing
将分支节点连接到当前的master结点(linear)
之后执行fast-forward,即可将分支结点与master连接,成为linear
```bash
git rebase master
```
如果产生冲突,则在mergetool中解决
```bash
git mergetool
git rebase --continue
```
终止rebase
```bash
git reabse --abort
```

## cherry picking
从分支中,拿出一个结点,作为master上的新节点(过程中没有合并)
```bash
git cherry-pick commit_id
```

## branch file add master
```bash
git restore --sourse=fixbug toc.txt
```
