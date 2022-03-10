# WSL安装过程



好久没有更新了...那么今天就来搞出个大招!---WSL!
<!--more-->

喜欢Ubuntu是必然的,不臃肿,安装环境还轻松快乐,我超爱这里的
但是生态一直是Ubuntu的问题,虽然适合开发,但是日常使用的话,会很麻烦,虽然也会有替代的东西可以使用,但是一般配置起来成本会很高,而且很多情况下,易用性也远远达不到win环境下的使用效率...所以经常就是双系统下来回切换...开机关机
之前听过学长说过wsl,但当时由于刚接触Ubuntu,所以认为原生系统当然是最好的,但经历了一个月的痛苦挣扎,我又想到了学长的一番话,打算试一试
去网上看了一下风评,感觉大多都不错,那就开始吧!
这里默认安装的是 wsl-2 版本
[官方教程](https://docs.microsoft.com/zh-cn/windows/wsl/install-win10)

# 安装Ubuntu子系统
## 启用wsl
```bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
## 开启虚拟机
```bash
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
**重启电脑**

## 下载安装子系统
windows store中安装Ubunut子系统

## 安装wsl2内核
[下载地址](https://docs.microsoft.com/zh-cn/windows/wsl/wsl2-kernel)

## 查看当前安装版本
```bash
wsl --list --verbose
```

## 升级至wsl2
```bash
wsl --set-version <distribution name> <versionNumber>
# <distribution name>: Ubuntu-20.04
# <versionNumber>: 2
# 示例: wsl --set-version Ubuntu-20.04 2
```
至此,安装wsl2完成,经过巨硬的集成,轻松又快乐

# VSCODE配置
vscode安装romote wsl插件
记得先换linux源,否则库下载太慢了

## 安装依赖库
```bash
sudo apt-get update
sudo apt-get install wget ca-certificates
```

## 安装vscode服务器
```bash
code .
```
在vscode中,拓展插件需要额外重新安装在wsl环境中,这个需要注意一下

# 配置C++环境
[官方教程](https://code.visualstudio.com/docs/cpp/config-wsl)
```bash
sudo apt-get update
sudo apt-get install build-essential gdb    # g++编译环境
```
## task.json
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "type": "shell",
      "label": "g++ build active file",
      "command": "/usr/bin/g++",
      "args": ["-g", "${file}", "-o", "${fileDirname}/${fileBasenameNoExtension}"],
      "options": {
        "cwd": "/usr/bin"
      },
      "problemMatcher": ["$gcc"],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    }
  ]
}
```

## launch.json
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++ build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}/${fileBasenameNoExtension}",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "g++ build active file",
      "miDebuggerPath": "/usr/bin/gdb"
    }
  ]
}
```

## c_cpp_properties.json
```json
{
  "configurations": [
    {
      "name": "Linux",
      "includePath": ["${workspaceFolder}/**"], //可设置外部库路径
      "defines": [],
      "compilerPath": "/usr/bin/gcc",
      "cStandard": "c11",
      "cppStandard": "c++17",
      "intelliSenseMode": "clang-x64"
    }
  ],
  "version": 4
}
```
最后wsl下的vscode也非常好用,让我属实有些被惊艳到了
buff加成: 效率*2
