# 搭建TF-GPU环境



搭建TF-GPU环境
<!--more-->

# conda命令

```bash
conda create -n <envname> python=x.x # 安装虚拟环境
conda remove -n <envname> --all # 删除虚拟环境
```

# 创建py环境

```bash
conda create --name py35_tensorflow python=3.7
activate py37_tensorflow
pip install tensorflow_gpu==2.5.0
```

# TF1安装keras-contrib

```bash
pip install git+https://www.github.com/keras-team/keras-contrib.git
```

# 镜像

```bash
清华
-i https://pypi.tuna.tsinghua.edu.cn/simple
豆瓣
-i https://pypi.doubanio.com/simple
```

# TF2 配置要求

```python
Tensorflow 2.5.0
Python 3.7
cuda 11.2
cudnn 8.1.1  
```

# 参考项目

```bash
https://github.com/saiwaiyanyu/bi-lstm-crf-ner-tf2.0
```




