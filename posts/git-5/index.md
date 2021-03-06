# gitignore配置



gitignore文件的使用
<!--more-->

## 刷新gitignore文件

```bash
git rm-r --cached .

git add .

git commit -m 'update .gitignore'
```

## 规则

常用的规则

```bash
/mtk/       # 过滤整个文件夹
*.zip       # 过滤所有.zip文件
/mtk/do.c   # 过滤某个具体文件
```

指定要将哪些文件添加到版本管理中

```bash
!src/        # 不过滤该文件夹
!*.zip       # 不过滤所有.zip文件
!/mtk/do.c   # 不过滤该文件
```

## 配置语法
以斜杠`/`开头表示目录；
以星号`*`通配多个字符；
以问号`?`通配单个字符
以方括号`[]`包含单个字符的匹配列表；
以叹号`!`表示不忽略(跟踪)匹配到的文件或目录；

此外，git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效；

## 示例说明

### 规则：

```bash
fd1/*
# 说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

/fd1/*
# 说明：忽略根目录下的 /fd1/ 目录的全部内容；

/*
!.gitignore
!/fw/bin/
!/fw/sf/
# 说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；
```
