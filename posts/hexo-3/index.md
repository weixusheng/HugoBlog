# NEXT Part-2



今天大部分时间在搞数学,所以更的有点少(哭...)
<!--more-->

# 404页面
看了很多帖子,最后都不好使,结果在官方文档上找到了...
直接将404.html文件放在"next/source"下就行啦

# 移动端背景图片模糊
因为[background-attachment](https://www.w3school.com.cn/cssref/pr_background-attachment.asp)属性在移动端上并不好使,所以移动端上的背景图片就糊成了一片
最后找到了一种妥协的方法,就是将图片进行repeat拼接
另外还有个知识点[before标签](https://www.w3school.com.cn/cssref/selector_before.asp)
```css
body:before {
	top: 1rem;
	left: 0;
	bottom: 0;
	right: 0;
	z-index: -1;
}
body {
      background: url(https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Blog_sourse/images/bg-common.jpg);
      background-size: cover;
      background-repeat: repeat;    //拼接
      background-position: 50% 50%;
}
```

# 增加对数学公式的支持
## 更换Hexo的markdown渲染引擎
hexo-renderer-kramed引擎是在默认的渲染引擎，hexo-renderer-marked的基础上修改了一些bug，两者比较接近，也比较轻量级。
```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```
## 修改文件
为了解决语义冲突,找到文件
```
node_modules\kramed\lib\rules\inline.js
```
修改如下两处
```
  //escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
  escape: /^\\([`*\[\]()#$+\-.!_>])/,

  //em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
  em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

## 开启mathJax
在next主题中开启mathJax开关
```
# Math Formulas Render Support
math:
  per_page: true
  engine: mathjax   
  mathjax:
    enable: true    # 这个改为true
    mhchem: true    # 这个改为true
```

## 文章中添加头标签
```
---
---title:  Using-Machine-Learning-for-Data-Center-Cooling-Infrastructure-Efficiency-Prediction
date: 2019-11-28 22:53:32
categories: [datacenter, machinelearning]
category: DataCenter
mathjax: true
---
```

# fancybox
进入next主题目录下执行
```
git clone https://github.com/theme-next/theme-next-fancybox3 source/lib/fancybox
```
然后在主题配置的_config.yml中
```
fancybox: true
```
