# Hexo迁移到ubuntu的坑


准备以后编程开发环境都转移到ubuntu上，这就涉及到以后blog的更新，想了想不如把hexo迁移到这里，原以为只是clone远程库就完事了，但没想到还是好多坑！吼！
<!--more-->

需要安装的工具
```
git	npm	nodejs

安装命令：
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install git
npm install -g hexo

安装完成后查看版本号：
nodejs -v
npm -v
```
**windows上可复制过来的文件**
```
sourse文件，_config文件，theme文件
```
<1>创建空文件夹，执行init hexo ，然后将sourse文件进行替换，之后hexo g生成文件 ，hexo s启动本地服务器命令就可以在默认landscape主题下启动hexo

<2>win与ubuntu两种环境下最大的区别就是，ubuntu需要手动去安装很多依赖库文件，例如在hexo d上传时需要执行npm install –save hexo-deployer-git

<3>最后安装admin模块方便增加删除文章 npm install hexo-admin

##### 使用hexo过程中的坑
hexo init后要安装依赖库
```
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked --save
npm install hexo-renderer-stylus --save
npm install hexo-generator-feed --save
npm install hexo-generator-sitemap --save
```
在使用定制theme时也要注意，是否需要额外的依赖库

