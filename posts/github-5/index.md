# Git master转为main


因为前段时间的BLM运动,Github就把master改名成了main,因为master有奴隶主的意思...

这就导致本地分支和main不统一,无法push
所以只能通过合并分支来解决
```bash
git checkout -b main
# Switched to a new branch 'main'
git branch
# * main
#  master
git merge master # 将master分支合并到main上
# Already up to date.
git pull origin main --allow-unrelated-histories 
# git pull origin main会报错：refusing to merge unrelated histories
git push origin main
```
