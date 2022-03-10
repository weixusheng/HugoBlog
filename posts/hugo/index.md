# 搭建Hugo


搭建Hugo
<!--more-->

# 配置环境

## 安装Go

去官网下载go

## 设置两个环境变量

`GOROOT` : 安装的文件夹`D:\Go`

`Path` : `D:\Go\bin`

## 安装hugo

```bash
scoop install hugo-extended
```

## 新增站点

```bash
hugo new site tronwei.top
```

## 安装主题

```bash
cd tronwei.top
git clone https://github.com/Lruihao/FixIt.git themes/FixIt
```

## 配置博客

配置文件为`config.toml`

文章目录为`posts`

## 部署

调试

```bash
hugo server
```

生成部署文件

```bash
hugo
```

# webhook自动化部署

## 创建github仓库

## 客户端

### 配置git

`public`文件夹下连接`github`仓库

```bash
git remote add origin ...
git commit
git push
```

### 配置deploy脚本

```bash
echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace with `hugo -t <YOURTHEME>`
# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back up to the Project Root
cd ..
```

### 配置github webhook

在仓库的`setting`中增加`webhook`配置

打开托管仓库`github.com/K1ngram4/myblog`点击右上角`Settings`按钮，选择`Webhooks`，点击右上角`Add wehook`

Payload URL 填服务器端创建好的`http://kingram.top:7777/webhook`

Content type选择`application/json`

Secret填入`mysecret`(该参数在服务器端的`github-deploy.sh`中配置)

## 服务器端

### 安装node

```bash
cd ~
wget https://nodejs.org/dist/v14.17.3/node-v14.17.3-linux-x64.tar.xz
tar -xvf node-v14.17.3-linux-x64.tar.xz
cd node-v14.17.3-linux-x64/bin
ln -s /root/node-v14.17.3-linux-x64/bin/node /usr/bin/
ln -s /root/node-v14.17.3-linux-x64/bin/npm /usr/bin/

# 验证安装
node -v
npm -v
```

### 安装pm2

```bash
npm install pm2@latest -g
ln -s /root/node-v14.17.3-linux-x64/bin/pm2 /usr/bin/

# 验证安装
pm2 -v
```

### 连接github仓库

```bash
git remote add origin ...
git pull
```

### 配置webhook脚本

```bash
cd ~
mkdir webhook
cd webhook
touch hugo-deploy.sh
vi hugo-deloy.sh
```

`hugo-deloy.sh`

```bash
#!/bin/bash

cd ~/myblog
git pull origin main
hugo 
echo "deploy success!"
exit 0
```

`增加执行权限`

```bash
chmod +x hugo-deploy.sh
```

### 配置ssh

```bash
cd ~/.ssh
ls
# 看是否存在 id_rsa 和 id_rsa.pub文件，如果存在，说明已经有SSH Key

# 否则生成ssh文件
ssh-keygen
cat ~/.ssh/id_rsa.pub
# 添加到github中
```

### 安装webhook脚本

```bash
# 安装
npm install github-webhook-handler --save
```

创建`github-webhook.js`文件

```javascript
var http = require('http')
var createHandler = require('github-webhook-handler')
var exec = require('child_process').exec
var handler = createHandler({ path: '/webhook', secret: 'mysecret' })

http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(7777)

console.log("github Hook Server running at http://0.0.0.0:7777/webhook");

handler.on('error', function (err) {
  console.error('Error:', err.message)
})

handler.on('push', function (event) {
  console.log('Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref)
    exec('sh ./hugo-deploy.sh', function (error, stdout, stderr) {
        if(error) {
            console.error('error:\n' + error);
            return;
        }
        console.log('stdout:\n' + stdout);
        console.log('stderr:\n' + stderr);
    });
})
```

`启动脚本`

```bash
node github-webhook.js # 调试模式

# pm2 管理进程启动
pm2 start github-webhook.js
pm2 startup
```

## 执行部署

```bash
# 在客户端的tronwei.top文件下执行
./deploy.sh
```

# 配置algolia搜索

## algolia流程

1. 创建`application`
2. 在`application`中创建索引`Indices`
3. 配置搜索属性: `searchable attribute`
4. 配置自定义排行: `title`

将`public`下的`index.json`上传

## 自动化上传

安装 atomic-algolia 包

```bash
npm install atomic-algolia --save
```

修改根目录下的 `package.json` 文件，在 `scripts` 下添加 `"algolia": "atomic-algolia"`

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "algolia": "atomic-algolia"
  }
}
```

根目录下新建 `.env` 文件，内容如下：

```bash
ALGOLIA_APP_ID= Application ID
ALGOLIA_INDEX_NAME= 索引(Indices)名字
ALGOLIA_INDEX_FILE= public/index.json
ALGOLIA_ADMIN_KEY= Admin API Key
```

上传命令

```bash
npm run algolia
```


