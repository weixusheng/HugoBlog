# VScode代码折叠


今天学c的时候,看到预处理命令#pragma,想起来之前用的代码折叠
既然创建了vscode专区,就把之前的技巧放在这里,方便查找
<!-- more-->

## vscode构造折叠块
```c
#pragma region "折叠块的名字"

//your code here

#pragma endregion
```

## vscode 代码折叠
**1. 折叠所有区域代码的快捷：**
ctrl + k &emsp; ctrl + 0 ;
先按下  ctrl 和 K，再按下 ctrl 和 0 ; ( 注意这个是零，不是欧 )

**2. 展开所有折叠区域代码的快捷：**
ctrl +k &emsp; ctrl + J ;
先按下  ctrl 和 K，再按下 ctrl 和 J
