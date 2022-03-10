# VScode同步设置


VScode虽然很棒,但是配置文件无法同步,这点有些麻烦,还好有插件支持同步
<!-- more-->

# 插件名字: Settings Sync
下面开始说配置过程
# 上传设置
## 生成 Github gist Token 
在Settings页面点击进入 Developer settings （开发者设置）进入Personal access tokens（个人授权令牌）页面生成一个令牌 
点击 Generate new token
令牌的作用能帮助我们就在VSCode中使用自己的私有令牌访问自己的保存在Gist上的配置,勾选Gist，点击生成。

## 创建Gist
创建一个新Gist,输入创建的Gist描述和片段内容("vscode-setting-sync")保存即可。
手动复制刚才创建的Gist仓库的ID：gist冒号后面的字符串,将其保存

## 配置VScode
### 填写Gist Token
回到VSCode编辑器中 使用快捷键Ctrl+P 输入命令 >sync 点击 同步：高级选项
然后选择同步："编辑配置设置"
输入你在github上创建的 gist token ,Ctrl+S保存更改
### 填写Gist ID
进入Settings Sync扩展设置页面设置
输入创建的Gist仓库ID 输入自动保存设置
### 上传
然后 ctrl shift p 调出快捷命令 sync upload

# 新的环境下下载设置
## 填写Gist ID
在新的环境下直接填写 gist id 即可
## 下载
然后调用 sync download 命令
