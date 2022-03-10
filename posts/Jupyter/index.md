# 关联Jupyter Notebook和conda



Jupyter Notebook和conda
<!--more-->

### 关联Jupyter Notebook和conda

安装`nb_conda`

```bash
conda install nb_conda
```

#### env环境错误的问题

更改文件：
` \anaconda\Lib\sitepackages\
 nb_conda\[envmanger.py]`

搜索其中的：
 ![img](https://www.pianshen.com/images/512/bcb005a0baad86d2212e5dbb02c40f58.png)
 将其改为：
 ![img](https://www.pianshen.com/images/163/bc415357ea2fc653a61b1b6a6d0e4f4b.png)
 重新打开Jupyter Notebook，问题得到解决。

### 修改Jupyter启示文件位置

打开 Anaconda Prompt输入

```text
jupyter notebook --generate-config
```

生成 Jupyter notebook 的配置文件

打开此文件`jupyter_notebook_config.py `

 找到c.NotebookApp.notebook_dir 这个变量

```markdown
1. 确保删除 “#”，取消这一行的注释模式。
2. 这一行代码前不能有空格。
3. 路径一定要是已经存在的，否则会闪退。且路径要用英文单引号括起来。
4. 路径要写\\转义, 或者在路径字符串前用r标识. 例:c.NotebookApp.notebook_dir = r'C:\Users\Administrator\Desktop\jupyter'
```




