# 快乐踩坑



这两天被苏菲上新装的win10系统折磨的够呛...踩了一堆坑<br>
cnpm国内镜像 | hexo d 报错
<!--more-->

# 微软商店500报错
可能是梯子和加速器配置文件冲突,最后莫名其妙把微软商店搞崩了...
之后把全网给出的方法都试了一遍,还是不行,硬着头皮重装系统了...
以后还是不开全局梯子了...怕了怕了

# hexo d 报错
```
$ hexo d -g
...(省略)
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object
```
错误原因在于nodejs版本过高
安装 "node 12.14" 版本即可

# cnpm国内镜像
以后再也不挂梯子了...老老实实用镜像吧
## 安装
```bash
npm install cnpm -g --registry=https://registry.npm.taobao.org
```

## win10下cnpm报错
```
cnpm : 无法加载文件 C:\Users\name\AppData\Roaming\npm\cnpm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
```
在 管理员模式powershell 下执行
```bash
set-ExecutionPolicy RemoteSigned
A
```
即可解决
cnpm真香!




