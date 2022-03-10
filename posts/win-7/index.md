# C++运行库安装报错(权限不足)



无法打开注册表项 UNKNOWN\Components\…
<!--more-->

今天安装MySQL的时候一直提示c++2019运行库安装失败
提示注册表权限不足,之前一直没有遇到这样的问题,google了好久也没有找到解决方法,在心态崩溃的边缘,最后终于找到了一篇博客(呜呜呜)
[参考blog](https://blog.csdn.net/dopdkfsds/article/details/81204286)

# 方法一 使用命令提示符解决
1. win+r打开运行对话框，输入cmd，管理员身份进入命令提示符
2. 执行以下命令
```bash
secedit /configure /cfg %windir%\inf\defltbase.inf /db defltbase.sdb /verbose
```
3. 运行完成重新安装


此方法只可解决部分人的问题，当出现下来情况时，表示此方法不起作用，使用第二个方法
```bash
任务已结束.在此操作期间,一些属性出现警告,可以忽略此警告
```

# 方法二 修改注册表权限
1. 使用组合键 Win+R 打开"运行"对话框，输入 regedit 并回车（需管理员权限）

2. 找到这个键值 ：
```bash
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData
```

3. 右击"UserData" 选择"权限(P)..."

4. 点击"高级(V)"按钮

5. 选定"Administrators……"，勾选**使用此对象继承的权限项目替换所有子对象的权限项目**
6. 点击"应用(A)"；将所有者更改为**Administrators**，注意不是Administrstor。

## 报错
如果在第5步：勾选-“使用此对象继承的权限项目替换所有子对象的权限项目”-点击应用

出现错误**注册表编辑器无法在当前所选的项及其部分子项上设置安全性**的弹窗

**解决方式**
1. 在以下链接中下载[psexec](https://docs.microsoft.com/zh-cn/sysinternals/downloads/psexec)

2. 下载好后解压

3. 使用**管理员权限打开命令提示符**(cmd)，定位到解压的文件夹执行以下命令进入注册表
```bash
psexec -i -d -s regedit
```
然后按照之前操作重复一遍即可

> **如何定位到解压文件夹**：
如解压文件夹为D:\xxx
在cmd窗口输入 "d:" 切换至D盘（注意要输入冒号）
输入cd 解压文件夹路径，即可
