# Hexo-百度谷歌收录



将网站内容收录入百度谷歌的搜索引擎
<!--more-->

# 百度
## 验证网站所有权
登录[百度站长平台](https://ziyuan.baidu.com/)
```
用户中心 –> 站点管理 –> 添加网站
```
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/hexo-5/1.png)
有 **文件验证、HTML标签验证、CNAME验证** 三种方式

这里选择的是**文件验证**这种方式。根据指示下载验证文件后，**将下载的文件放在博客根目录下的 source 文件夹下**，如果你下载的验证文件类型是 html 文件则**还需要对该 html 文件做相应修改**以保证该文件上传到网站后是一模一样的，即不被渲染和压缩。因此需要在 html 文件第一行加入下面的内容：
```html
---
layout: false
---
```
接着重新部署更新自己的博客后，确认验证文件是否可以正常访问,如果可以访问的话会出现之前下载的验证文件里的信息（即一串字母数字的组合），点击“完成验证”按钮即可。

## 选择提交方式
当网站通过验证之后，我们就可以使用链接提交工具了，目前有 api, sitemap、手动提交 三种方式。三者都是将站点自身的 URL 自动推送至百度，而后等待百度爬虫进行对页面的抓取。这里只说 api 和 sitemap方法...手动提交太慢了

## sitemap 提交
Sitemap（即站点地图）就是网站上各网页的列表。创建并提交 Sitemap 有助于百度发现网站上的所有网页。
### 安装 Sitemap generator 插件
**安装应在 hexo_blog 文件夹下安装,否则插件会无效**
```bash
npm install hexo-generator-baidu-sitemap --save
```
### 修改配置文件
打开博客站点配置文件 _config.yml，增加 baidusitemap 属性：
```bash
baidusitemap:
    path: baidusitemap.xml
```
这样执行 hexo g 时，会在 public/ 文件夹下生成站点文件 baidusitemap.xml。
## 填写地址
填写site.xml文件所在的地址
```bash
blog域名/baidusitemap.xml
```

## api 提交
这个提交时在 hexo-d 时执行,将新生成的网站推送到 baidu站台
例如当前调用地址为
```bash
http://data.zz.baidu.com/urls?site=tronwei.top&token=demotoken123
#host: tronwei.top
#token: demotoken123 
```
### 安装插件：
```bash
npm install hexo-baidu-url-submit --save
```
最后，打开博客站点配置文件 _config.yml，增加 baidu_url_submit 属性以及修改 url 属性和 deploy 属性：
```bash
# 百度主动推送
baidu_url_submit:
  count: 5 ## 提交最新的 5 个链接
  host: tronwei.top ## 注意修改为你在百度站长平台中注册的域名
  token: demotoken123 ## 注意修改为你的秘钥
  path: baidu_urls.txt ## 文本文档的地址  新链接会保存在此文本文档里

deploy:
  - type: git
    repo:
     github: https://github.com/weixusheng/weixusheng.github.io.git
     gitee: https://gitee.com/tronwei/tronwei.git
    branch: master
  - type: baidu_url_submitter # new line
```
这样执行 hexo generate 时，会在 public/baidu_urls.txt 这个文本文件保存最新的链接
执行 hexo d 时，会从上述文件中读取链接提交到百度站台

# 谷歌收录
## 验证网站
与百度收录操作相似，但是谷歌的效率要高很多，操作完之后基本就收录完成了。
添加网站进行验证
打开[谷歌搜索控制台](https://search.google.com/search-console/welcome)，选择 URL prefix，输入你的博客域名后进行验证，这里使用的仍然是 HTML 验证方式。与百度收录操作相似的，下载 html 文件到博客根目录下的 source 文件夹并在 html 文件前加上代码：
```html
---
layout: false
---
```
重新部署更新自己博客后，点击 VERIFY 按钮查看是否添加验证成功。

## 提交 Sitemap
安装插件：
```bash
npm install hexo-generator-sitemap --save
```
打开博客站点配置文件_config.yml，增加 sitemap 属性：
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/hexo-5/2.png)
```bash
# googleSiteMap
sitemap:
  path: sitemap.xml
```
执行 hexo g 时，会在 public/ 文件夹下生成站点文件 sitemap.xml。执行 hexo deploy 时，将上述文件部署到云端。

