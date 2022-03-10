# VScode配置C环境



建立Linux下的C环境
<!-- more-->

> 2020.6.22更新
## 关于math.h等头文件出现链接错误
修改task.json文件
```
"args": [
    "-g",
    "${file}",
    "-lm",  //新增项
    "-o",
    "${fileDirname}/${fileBasenameNoExtension}"
],
```

## 安装环境
gcc/g++编译程序和gdb调试程序
```c
sudo apt-get install gcc     
sudo apt-get install g++
sudo apt-get install gdb
```

## 插件法
安装插件
```
C++ Intellisense
C/C++ Compile Run
```
F6或F7即可编译运行

## 配置法
在代码界面F5运行，vscode提示选择调试程序，选择gdb
然后选择模板,这里我选择的是"gcc-9"
此时自动生成 launch.json文件
之后继续F5,自动生成 task.json文件
至此配置完成



