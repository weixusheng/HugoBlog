# Shell-字符串正则提取


正则表达式
我吐辣...正则和shell真是折磨王了
从昨晚折磨我到今天上午
还好写出来了,要是再搞不出来真要自闭了
```bash
$ for x in *;do var=$(echo $x|egrep -o '[^img_.png]');var=$((1+$var)).png; mv $x $var; done
```
<!--more-->

**正则表达式**
表示筛选除去这些字符以外的字符,即想要提取的数字
```bash
[^img_.png] 
```
**egrep命令**
```bash
$x|egrep -o '[^img_.png\]' 
```
返回的是匹配数量,并且打印出来匹配的字符
如果想要赋值给变量var,需要使用
```bash
$(echo)
```
在shell中进行数值运算需要用到
```bash
$((表达式))
```
并且在shell中进行赋值时,'='两侧不可以加空格
