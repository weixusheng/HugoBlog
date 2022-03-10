# Shell语法


Shell教程

<!--more-->

# 碎碎的知识点
## 批量重命名
之前在ubuntu的时候,能用bash直接批量处理图片命名
(因为每次查资料截图都会产生大量的图片,之前手动一个个命名真是太顶了
因为wsl的出现,所以现在还是回到了win的怀抱
但是在cmd下,使用之前的Linux下的bash命令,会产生奇怪的错误,在Linux下,图片命名是按照时间产生的顺序排序,正好符合我的意思
但是在windows下没有按照时间顺序命名,每次都是奇奇怪怪的顺序,导致命名后的文件都没有办法使用
懒得每次都调用py文件之类的,想直接在控制台中写好,所以只好乖乖去学shell啦

```shell
$ for x in *; do mv $x ${x:4}; done
```
其中
```shell
${x:4} #从x变量中的第4号位置开始截取字符串(第一个位置为0号)
```

## 数据写入文件
```shell
echo "hello">>text.txt #将hello写入text.txt文件中
```

## tree命令
```shell
tree /f video #/f命令为查看所有文件名字(video为文件名)
```

## 打开Windows terminal
```shell
wt -d . # 当前目录打开Windows terminal
```

## seq关键字
时间倒计时.sh
```shell
echo -n "Time: "
for i in `seq 9 -2 1`
	do 
		echo -n -e "\b$i"
		sleep 1
done
echo
```
seq基本用法
seq ... 尾数(一个变量)
seq ... 首数 尾数(两个变量)
seq ... 首数 增量 尾数(三个变量)
**以指定增量从首数开始打印数字到尾数**

# Shell教程
## 脚本编写规范
一般在第一行填写基本信息
```shell
#！/bin/bash 

# Date：16:29 2018-10-20 
# Author: Create by xiaoxie
# Description: This script function is …… 
# Version： 1.1 
```

## 变量
1. **交互式读取**
```shell
read -p "提示信息" 变量名
```

2. **赋值时使用引号的作用**
 - 双引号：允许通过$符号引用其他变量值
 - 单引号：禁止引用其他变量值，$视为普通字符
 - 反撇号：命令替换，提取命令执行后的输出结果 

全局变量的定义方法 export 变量名

3. 预定义变量
预定义变量和环境变量相类似，也是在Shell一开始就定义的变量，不同的是，用户只能根据shell的定义来使用这些变量，所有预定义变量都是由符号“$”和另一个符号组成。 常见的Shell预定义变量有以下几种。
 - $# ：位置参数的数量
 - $* ：所有位置参数的内容
 - $? ：命令执行后返回的状态，0表示没有错误，非0表示有错误
 - $$ ：当前进程的进程号
 - $! ：后台运行的最后一个进程号
 - $0 ：当前执行的进程名

例如 example.sh 文件
```shell
echo $1
echo ${2}+${3}
# 执行bash example.sh 10 11 12

#输出两个参数
#10
#23
```

## 常见运算符
### (())
用于整数运算的常见运算符,效率很高
```shell
((i= i+1)) #i增加1
i= $(($a+5)) #i = a变量+5
```

### let
用于整数运算
```shell
let i = i+8
```

### expr
expr直接可计算出结果
配合变量计算
```shell
i = `expr $i+5` #将i变量+5 赋值给i
```

### []
可用于整数的运算
```shell
echo $[2-5] #得到-3
```

## 逻辑语句
### if-then
```shell
if condition
    then commands
else 
    commands
fi
```

### if-elseif
```shell
if conditon
    then commands
elseif condition
    then commands
else
    commands
fi
```

### case
```shell
case variable in
    v1)
        commands
    ;;
    v2)
        commands
    ;;
    v3)
        commands
    ;;
    *)
        commands
esac
```

举例
```shell
read -p "please input a num or str:" i
case $i in
    [1-9])
        echo "this is a number"
    [a-z])
        echo "this is a letter"
    *)
        echo "this is shit"
esac
```

### for
```shell
for conditon
    do 
        commands
done
```

### while
```shell
while condition
    do
        commands
done
```

## 函数
```shell
function_name(){
    commands
    return N
}
```
函数调用直接使用名字即可(不需要加双括号)

# Linux三剑客
 - grep擅长查找功能
 - sed擅长取行和替换
 - awk擅长取列

## 正则表达式
[参考博客](https://www.cnblogs.com/zimskyzeng/p/11630071.html)

### 元字符
**.** <kbd>匹配任意单个字符</kbd>
**[]** <kbd>匹配指定范围内的任意单个字符</kbd>
__[^]__ <kbd>匹配指定范围外的任意单个字符</kbd>

### 字符集合
**[[:digit:]]** <kbd>匹配单个数字</kbd>
**[[:lower:]]** <kbd>匹配单个小写字母</kbd>
**[[:upper:]]** <kbd>匹配单个大写字母</kbd>
**[[:punct:]]** <kbd>匹配单个标点字符</kbd>
**[[:space:]]** <kbd>匹配单个空白字符</kbd>
**[[:alpha:]]** <kbd>匹配单个字母</kbd>
**[[:alnum:]]** <kbd>匹配单个字母或数字</kbd>

### 匹配次数（贪婪模式）
__*__ <kbd>匹配其前面的字符任意次</kbd>
**?** <kbd>匹配其前面的字符0次或者1次</kbd>
**+** <kbd>匹配其前面的字符至少1次</kbd>
__.*__ <kbd>任意长度的任意字符</kbd>

### 位置锚定
__^__ <kbd>锚定行首，此字符后面的任意内容必须出现在行首</kbd>
__$__ <kbd>锚定行尾，此字符前面的任意内容必须出现在行尾</kbd>
__^$__ <kbd>空白行</kbd>

### 实际使用
由于linux系统shell解释器的特殊处理，某些元字符在linux下具有展开式等特殊含义，在实际的使用过程中我们需要**添加 \ 进行转义**

__\\?__ <kbd>匹配其前面的字符1次或0次</kbd>
__\\+__ <kbd>匹配至少一次</kbd>
__\\{m,n\\}__ <kbd>匹配其前面的字符至少m次，至多n次</kbd>
__\\{1,\\}__ <kbd>匹配前面的字符至少1次</kbd>
__\\{0,3\\}__ <kbd>匹配前面的字符0次至3次均可</kbd>

>备注：至少0次，必须要显式的写出来；

__\\<__ <kbd>锚定词首，其后面的任意字符必须作为单词首部出现</kbd>
__\\>__ <kbd>锚定词尾，其前面的任意字符必须作为单词的尾部出现</kbd>

### 分组与后向引用
\\(\\)
\\(ab\\)*
\\1：<kbd>引用第1个左括号以及与之对应的括号所包括的所有内容</kbd>
\\2：<kbd>引用第2个左括号以及与之对应的括号所包括的所有内容，以此类推</kbd>

## 拓展正则表达式
可以看到标准正则表达的使用过程中，许多符号都需要转义，这在工作中带来了一定的不便，因此扩展的正则表达式便出现了。

### 字符匹配：
__.__ <kbd>匹配单个字符</kbd>
__[abc]__ <kbd>包含abc任意一个字符</kbd>
__[^abc]__ <kbd>不包含abc任意一个字符</kbd>

### 次数匹配（不用再转义）：
__*__ <kbd>匹配前一个字符任意次</kbd>
__?__ <kbd>匹配其前面的字符1次或0次</kbd>
__+__ <kbd>匹配其前面的字符至少1次</kbd>
__{m,n}__ <kbd>匹配其前面的字符至少m次，至多n次</kbd>

### 位置锚定：
对比使用方式：__^__ 和 __$__
这里要注意的是对于词首定位和词尾定位，分别是 __\\<__ 和 __\\>__ ，依然需要加上反斜杠；

### 分组（不用再转义）：

()：分组
\1, \2, \3：<kbd>分别对应第n个括号所匹配的内容</kbd>
或运算

__|__ <kbd>可以同时取并集</kbd>
> 注意：C|cat表示的是C或cat（表示的是整个部分）
可以看到，**使用扩展的正则表达式可以省略很多的转义符号**，这尤其在写sed语句时极大的提高了代码的可读性。建议优先使用扩展的正则表达式

# grep命令家族
grep命令家族由grep, egrep, fgrep 三个子命令组成，适用于不同的场景。具体如下：

 - grep 原生的grep命令，使用“标准正则表达式”作为匹配标准。
 - egrep 扩展的grep命令，相当于$(grep -E)，使用“扩展正则表达式”作为匹配标准。
 - fgrep 简化版的grep命令，不支持正则表达式，但搜索速度快，系统资源使用率低。

## 语法
```shell
grep [options] PATTERN [FILE...]
```

### options部分
**-i** 忽略大小写
**--color** 高亮匹配上的字符串
**-v** 显示没有被模式匹配到的行
**-o** 只显示被模式匹配到的字符串
**-E** 使用扩展的正则表达式，egrep=grep -E

### PATTERN部分
以字符串的方式给定匹配模板，可以使用普通字符串以及正则表达式(标准&扩展)

### FILE部分
需要查找内容的文件

# sed命令
sed全称为Stream EDitor，sed是一个**流编辑器**，在处理行内容时功能十分强大

## 基本语法
```shell
sed [option] 'script' [input file]...
```

### option部分
-n：不输出模式空间中未匹配上的内容stdout，详情可以看4.3高级用法；
-e：可以在sed命令中指定多个script脚本，多点编辑功能；
-f：输入sed脚本，脚本中写着编辑命令；
-r：支持使用扩展的正则；
-i：直接编辑源文件；

### script部分
script部分包含两个内容，其一是定界，即确定我们要操作的范围。另一个内容是操作，比如替换、插入当前行或在后面插入等操作。
1. 定界-空地址
即全文编辑；

2. 定界-单地址：
n：指定第n行，对特定行进行编辑；举例：sed -n '1p' passwd 仅输出第一行；
/pattern/：指定模式匹配到的那一行，注意这里的pattern不是扩展正则表达式，如果要用扩展的正则表达式，需要在option需要使用-r；举例：sed -n '/sbin/p' passwd 输出能够匹配上sbin的内容行；

3. 定界-范围：
**n,m**：定位从第n行开始至第m行(都是闭区间)；
**n,+k**：定位从第n行开始，包括往后的k行；
**n,/pattern/**：定位从第n行开始，至指定模式匹配到的那一行；
**/pattern1/,/pattern2/**：定位从pattern1模式匹配开始，直到pattern2模式匹配之间的范围；

4. 定界-步进方式：
1~2：以1为起始行，然后步进2行向下匹配，即所有的奇数行；
2~2：以2为起始行，然后步进2行向下匹配，即所有的偶数行；

5. 编辑操作：
 - **d** 删除整行，d放在定界后面。举例：
```shell
sed '/sbin/d' passwd；
```

 - **p** 显示模式空间中的内容, p放在定界后面。一般来说，p操作和-n选项配合使用，筛选出我们匹配的行。若不加-n的话，由于模式空间中未匹配上的行也会输出，我们会发现所有内容都输出并且匹配行会输出2次；

 - **a** 在匹配的行后面增加文本，使用\n支持多行追加，a放在定界后面。举例：
```shell
sed '1a first_line\nsecond_line' passwd 
```
在第1行后面插入两行内容(first_line 和 second_line)；

 - **i** 在匹配的行前面增加文本，i放在定界后面。举例：
```shell
sed '3i hello' passwd；
```

 - **c** 替换匹配行为指定的文本。举例：
```shell
sed '/root/c text' passwd 把匹配到的行替换成text；
```

 - **w** 保存模式空间中匹配的内容到指定位置。举例：
```shell
sed -n '/^[^#]/w /tmp/demo' /etc/fstab 
```
即将/etc/fstab中非#开头的行输出保存到/tmpdemo中。

 - **r** 读取指定文件的内容添加到当前文件匹配到的行后面，进行文件合并。举例：
```shell
sed '2r /etc/passwd' 1.txt 
```
即将/etc/passwd文件的内容读取，并插入到1.txt文件的第二行处。

 - **!** 条件取反 用法：地址定界!编辑命令。

 - **s///** 条件替换 这里的/可以用其他特殊符号，其替换分隔符的判定标准是s字符后的第一个特殊符号。这在替换文本中涉及特殊符号时特别好使，我们避开需要替换的特殊符号即可。即
```shell
sed 's@root@me@g' /etc/passwd 等同于 sed 's/root/me/g' /etc/passwd。
```

>替换标记备注：g(全局替换)，p(显示替换成功的行)
替换举例：根据输入查找目录，下面输出的是/var/log/
```shell
echo "/var/log/messages" | sed 's@[^/]\+$/\?@@'
```

## sed高级用法
<img loading="lazy" src="https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/shell/1/1.png">
在模式空间中，完成匹配的操作。当没有匹配上的时候，文本行内容会默认输出stdout；当匹配上文本行的时候，会执行编辑命令，执行结果输出到stdout中
保持空间可以理解为一个暂存区，只是用于完成额外的动作

### 相关参数
h：把模式空间中的内容覆盖至保持空间中；
H：把模式空间中的内容追加至保持空间中；
g：把保持空间中的内容覆盖至模式空间中；
G：把保持空间中的内容追加至模式空间中；
x：把模式空间中的内容与保持空间中的内容互换；
n：覆盖读取匹配到的行的下一行(改变指向)至模式空间中；
N：追加读取匹配到的行的下一行(改变指向)至模式空间中；
d：删除模式空间中的行；
D：删除多行模式空间中的所有行；

### 举例
**sed -n 'n;p' FILE**：显示偶数行；
**sed '1!G;h;$!d' FILE**：逆序显示文件的内容；
**sed '$!d' FILE**：取出最后一行；
**sed '\$!N;$!D' FILE**：取出文件后两行；
**sed '/^$/d;G' FILE**：删除原有的所有空白行，而后为所有的非空白行后添加一个空白行；
**sed 'n;d' FILE**：显示奇数行；
**sed 'G' FILE**：在原有的每行后方添加一个空白行；

### 基础面试题举例
**面试题1：提取字符串中的指定字符串**
```shell
/bin/bash
info="hellozimskyshenzhen"
echo $info | sed 's/hello\(\w\+\)shenzhen/\1/g'
```
>注意事项：
sed中不支持\d，如果要用数字用[0-9]，但是支持\w。
在一般正则表达式中，sed中的()要转义，+要转义，<>要使用\转义。如果不想这么麻烦，那就加上-r选项使用扩展正则表达式吧！

**面试题2：判断输入是否为整数**
```shell
#!/bin/bash
if [ -n "$(echo $1 | sed -n '/^[0-9]\+$/p')" ] ; then
  echo 'yes'
else
  echo 'no'
fi
```

# awk命令
awk是发明该工具三个作者姓名的首字母简称，awk是一个报表生成器，主要用于**格式化输出**,格式化文本输出器

# 参考文献
 - [1][Linux三剑客及使用介绍](https://www.cnblogs.com/zimskyzeng/p/11630071.html)
 - [2][Linux三剑客(grep、sed、awk)](https://blog.csdn.net/sj349781478/article/details/82930982)
 - [3][linux中grep命令的用法](https://www.cnblogs.com/flyor/p/6411140.html)
 - [4][linux 命令之seq](https://blog.csdn.net/u011641885/article/details/45066661)
