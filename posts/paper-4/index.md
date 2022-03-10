# Word论文格式


论文格式的问题主要集中在目录，页眉，页脚三大部分
这里分这三块来讲
```
版本: Office 365 Word
```
<!--more-->

# 目录
目录很好生成，在引用中就可以一键生成，但生成的行距会有点问题，需要自己调整一下
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/1.png)

# 参考文献引用
首先将参考文献加到文章末尾,然后使用交叉引用
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/2.png)

# 文章分节
在论文中,我们会有不同的章节,而不同的章节会有不同的页眉
这就需要用到"分隔符"
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/3.png)
在章节的末尾加上分隔符-下一页,这样就可以将论文分节

# 页眉
页眉有三个重要的属性
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/4.png)
 - 首页不同
 - 奇偶页眉
 - 链接上一节

## 首页不同:
首页特别为单独一节,页眉页脚独立

## 奇偶页眉:
奇偶页码的页眉有所不同,偶数页码的页眉为相同值,奇数页码页眉为相同值
以之前设置的分隔符所划分的节,进行章节区分
即每个章节可以设置单独的页眉

## 链接到上一节
即当前节的页眉与上一节的页眉相同

# 页脚
## 格式设置
设置页码,首先需要进行页码格式设置
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/5.png)
选择样式
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/6.png)

## 不同部分页码
在文章中,我们可能需要前一部分"摘要,目录"与后一部分"正文,参考文献,致谢"两部分的页码区分,即两套页码
这时就需要在第二部分的页码,设置为"不链接到上一节"将其设置为单独的一节
然后设置页码为第一页,这样该部分就完成了页码设置
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/7.png)
加入页码
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/8.png)

# 生成的目录中有多余横线
如果在页码格式中设置为 -i- 类型
那么最后生成pdf时,目录页码也会变成 -i- 类型
解决方法为: 页码格式选择 i,之后手动在每个页的页码左右上加上"-",这样生成目录的页码就没有问题
[参考blog](https://blog.csdn.net/guolinlin11/article/details/51685625)

# 三线表的制作
右键点击表格的左上角图表
调出菜单,选择"表格属性"
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/9.png)
选择"居中",之后选择"底纹"
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/10.png)
选择线段位置和粗细
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/11.png)
选择第一行,调整三线表的第二条线
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/paper-4/12.png)
之后改变底边线段粗细,并应用于"单元格"
至此完成三线表

