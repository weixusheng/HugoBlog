# NEXT Part-1


18号刷PAT,19号改NEXT的bug,鸽了两天,今天全都补上
一直犹豫要不要换到NEXT,因为工作量很大,但是原来使用的主题拓展性实在太差了...我真的很想加个站内检索...因为经常需要找原来blog记录的某一个知识点
```
Version: NEXT-7.8 
```
<!--more-->

**以下方法使用于scheme: Gemini**
# 安装
仓库地址:
[NEXT](https://github.com/theme-next/hexo-theme-next)

# 首页不显示全文
next默认首页文章不显示全,但需要在markdown中进行标注
在需要显示的文字下增加,进行截至标注
```
<!--more-->
```

# 页面元素设置
具体参考[大佬的blog](https://blog.csdn.net/nightmare_dimple/article/details/86661502)
另外config文件中已经注释的很清楚了,慢慢看就可以完全理解

# 文章目录的坑
之前的主题,为了在文章前面加上目录,安装了"hexo-toc"插件
但是我一直以为是hexo自带的...使用next之后,文章目录显示错误,目录上没有文章锚点,并且不可以跟随阅读进度进行实时更新
大晚上的...这个bug差点没给我送走
还好最后搜到了一个[issue](https://github.com/theme-next/hexo-theme-next/issues/380)
万万没想到啊...这都可以,行吧,卸载就完事了
```
npm remove hexo-toc --save
```

# hexo g报错
出现如下错误:
```
Cannot read property 'replace' of null
```
原因为文章头部格式出错
[参考blog](https://blog.csdn.net/qq_36408085/article/details/104323674)

# 引入自定义css
NEXT-7取消了custom.styl文件,老版本的教程都无效
但是在config文件中进行了集成
```bash
custom_file_path:
  #head: source/_data/head.swig
  ...省略
  #mixin: source/_data/mixins.styl
  style: themes/next/source/_data/styles.styl #取消注释
```

在路径"themes/next/source"创建"_data"文件夹
创建styles.styl文件
我加入了以下的样式,感觉效果还不错:
 - 添加背景图片
 - 标题栏背景颜色
 - 透明度调整
 - 增加圆角

```
// 添加背景图片
body {
      background: url(https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/Blog_sourse/images/bg.jpg);//自己喜欢的图片地址
      background-size: cover;
      background-repeat: no-repeat;
      background-attachment: fixed;
      background-position: 50% 50%;
}

// 标题栏背景
.site-meta {
    padding: 20px 0;
    color: #fff;
    background: $black;
    background-repeat: no-repeat;
    background-attachment:fixed;
    background-position:center;
    background-size:cover;
}

//文章内容的透明度设置
.content-wrap {
  opacity: 0.94;
}

//侧边框的透明度设置
.sidebar {
  opacity: 0.86;
  border-radius: 40px 40px 0px 0px;
}

.sidebar-inner{
  border-radius: 40px 40px 0px 0px;
}

//菜单栏的透明度设置
.header-inner {
  background: rgba(255,255,255,0.9);
  border-radius 40px;
}

//搜索框（local-search）的透明度设置
.popup {
  opacity: 0.9;
}
```
[参考blog](https://blog.csdn.net/kuashijidexibao/article/details/106373443?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

# 标签页加入tagcloud
之前主题我在tag页中加入了文字云,感觉显示效果很棒!
然后就想在next中也加入,不过之前是pug模板,现在是swig模板,所以有点差别
然而最后为了省时间反而踩了个大坑(哭)
**请不要使用"hexo-tag-cloud"插件**
这里介绍我的方法,显示效果没问题,那个插件问题就很多...蜜汁默认300*150像素,我自适应大小后,云图直接糊了(我人傻了)...凌晨一点又差点给我送走,还好过了一晚上,今天决定用之前的了,之后就成功啦
但这个插件也能用,适合一个小块显示,不适用于整个页面显示[参考blog](https://blog.csdn.net/aoman_hao/article/details/89416634)
## 增加css代码
在之前的style.styl文件中加入
```
//标签云图css
.tag_box{
  position: relative;
  width: 100%;
}

.leancloud-visitors-count{
  margin-left: 5px;
}
.by_farbox {
  font-size: 10px;
}
.by_farbox a {
  color: #A6A6A6;
}
.by_farbox a:hover {
  color: #4786D6;
}
```

## swig增加代码
在next/layout/page.swig中
```
{%- if page.type === 'tags' %}
```
后增加tag-cloud代码
```
      <div class="post-body{%- if page.direction and page.direction.toLowerCase() === 'rtl' %} rtl{%- endif %}">
        {%- if page.type === 'tags' %}
        {#
	        新增加的云图显示标签页
	    #}
	    <style type="text/css">
     	 #resCanvas {
        	height: 100%; width: 100%;
     	 }
   	    </style>
   	    <script type="text/javascript" charset="utf-8" src="../js/tagcloud.js"></script>
   	    <script type="text/javascript" charset="utf-8" src="../js/tagcanvas.js"></script>
	    <div class="tag_box">
		    <div id="myCanvasContainer" class="widget tagcloud" style="text-align:center">
		     <canvas id="resCanvas">
			    {{ list_tags() }}
		     </canvas>
		</div>
	</div>
```

### Html5中的Canvas宽度为100%
这里的canvas自适应大小,参照这个blog
[参考blog](https://blog.csdn.net/u013682842/article/details/50328471)

# 内容块圆角
我找了好久的post-block(内容块的class),最后在对应的scheme文件中找到了
文件路径: "next/source/css/_schemes/Gemini/index.styl"
```
// Post & Comments blocks.
.post-block {
  background: var(--content-bg-color);
  border-radius: $border-radius-inner; //border-radius-inner变量
  box-shadow: $box-shadow-inner;
  padding: $content-desktop-padding;

  // When blocks are siblings (homepage).
  & + .post-block {
    border-radius: $border-radius; //border-radius变量
    // Rewrite shadows & borders because all blocks have offsets.
    box-shadow: $box-shadow;
    margin-top: $sidebar-offset;
  }
}
```
可以看到圆角值由两个css变量控制
打开文件: "next/source/css/_variables/Gemini.styl"
修改两个变量
```
$border-radius-inner     = 30px;
$border-radius           = 30px;
```
最后完成圆角修改

# 增加站内搜索
## 安装插件:
```
npm install hexo-generator-searchdb --save
```
## 修改站点文件_config.yml
添加如下内容：
```
# Search 
search:
  path: ./public/search.xml
  field: post
  format: html
  limit: 10000
```
 - path：索引文件的路径，相对于站点根目录
 - field：搜索范围，默认是 post，还可以选择 page、all，设置成 all 表示搜索所有页面
 - limit：限制搜索的条目数

[hexo增加search(非next主题)参考blog](https://cndrew.cn/2020/04/07/local-search/)

## 修改主题配置文件_config.yml
```
local_search:
  enable: true
```

# 文章字数统计、阅读时长
## 安装插件：
```
npm install hexo-symbols-count-time --save
```

## 修改主题配置文件_config.yml
```
symbols_count_time:
  separated_meta: true  # false会显示一行
  item_text_post: true  # 显示属性名称,设为false后只显示图标和统计数字,不显示属性的文字
  item_text_total: true # 底部footer是否显示字数统计属性文字
  awl: 4                # 计算字数的一个设置,没设置过
  wpm: 275              # 一分钟阅读的字数
```

## 站点配置文件_config.yml
增加以下内容
```
symbols_count_time:
#文章内是否显示
  symbols: true
  time: true
#网页底部是否显示
  total_symbols: true
  total_time: true
```
[参考blog](https://tding.top/archives/42c38b10.html)
