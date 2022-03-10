# PAC代理规则



编辑 PAC.txt 代理规则
<!--more-->

# 通配符支持
```shell
.example.com/ 
```
实际书写时可省略*如 .example.com/ 意即 *.example.com/

# 正则表达式支持
以\开始和结束， 如 [\w]+://example.com\

# 例外规则 @@
```shell
@@.example.com/ 
```
满足@@后规则的地址不使用代理
即所有example .com域名下的网址都不走代理

# 匹配地址开始和结尾 |
```shell
|http://example.com
example.com|
# 分别表示以http://example.com开始和以example.com结束的地址
# 意思是以http://example.com开始和以example.com结束的地址都走代理
```

# 标记||
```shell
||example.com
# http://example.com、https://example.com、ftp://example.com等地址均满足条件，只用于匹配地址开头
```

# 注释 ! 
```shell
! Comment
```

# 分隔符^
表示除了字母、数字或者 _ - . % 之外的任何字符。如
```shell
http://example.com^
# http://example.com/和http://example.com:8000/均满足条件，而http://example.com.ar/不满足条件
```
