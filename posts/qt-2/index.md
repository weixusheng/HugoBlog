# Pyside2程序打包


Pyinstaller实现程序打包
<!--more-->

这个打包程序,足足耗费了我两天的时间
以为一行命令就行,但是打包之后的程序,要么缺少东西,要么在新环境下缺少qt库,快给我搞自闭了,添加各种plugin和platform下的文件都不好使
直到昨天晚上,我才在博客的海洋中,找到有人提了一下Anaconda环境下打包会出问题,需要额外配置一下虚拟环境
我这才去试了一下,但是在windows terminal中一直调用的还是base环境下的包,就在最后放弃的关头!
看到有大佬在pycharm的terminal中调用出了虚拟环境,最后终于打包成功!

# Pyinstaller命令
|              命令               |                  解释                  |
| :-----------------------------: | :------------------------------------: |
|               -F                |          打包为一个文件(exe)           |
|           --workpath            |   指定了制作过程中临时文件的存放目录   |
|           --distpath            | 指定了最终的可执行文件目录所在的父目录 |
| --runtime-hook="runtimehook.py" |         库文件存在一个文件夹中         |
|         --noconsole(-w)         |         运行时没有terminal黑框         |
|        --icon="star.ico"        |                程序图标                |
|  --hidden-import PySide2.QtXml  |               导入动态库               |
**demo打包命令**
```python
pyinstaller main_run.py -w  --icon="star.ico" --hidden-import PySide2.QtXml
```
**其中 --hidden-import PySide2.QtXml 必要参数**

## 解决缺少"shiboken2.abi3.dll"文件
```bash
-p D:/Anaconda/envs/py37_env/Lib/site-packages/shiboken2
```

## 指定生成文件路径
```bash
--distpath E:/dist
--workpath E:/building_file
```

# 配置Python虚拟环境
在Anaconda中配置一个python3.7的环境
## conda常用命令
```python
conda list #查看安装了哪些包。
conda env list 或 conda info -e #查看当前存在哪些虚拟环境
conda update conda #检查更新当前conda
```

### 创建虚拟环境
```python
conda create -n xxx python=3.7
```
xxx为虚拟环境的名字,该文件可在Anaconda安装目录 envs文件下找到

### 激活虚拟环境
```python
activate your_env_name # Windows
source activate your_env_nam # Linux
```

### 关闭虚拟环境
```python
deactivate env_name # Windows
source deactivate   #Linux
```

### 指定环境管理包
```python
conda install --name $your_env_name  $package_name 
conda remove --name $your_env_name  $package_name 
```

### 删除虚拟环境
```python
conda remove -n your_env_name --all
```

最后切换到虚拟环境安装pyside2等库,实现打包

# 打包完成的程序结构
最后打包文件dist中应该有
 - 界面ui文件
 - 主程序exe文件
 - 程序图标ico文件

