# centos安装conda


整点没用的
<!--more--> 

# 下载
```bash
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-2021.11-Linux-x86_64.sh --no-check-certificate
```

# 安装
```bash
bash Anaconda3-2020.11-Linux-x86_64.sh

...
Please, press ENTER to continue
# 按enter 出现用户协议 一直按enter
Please answer 'yes' or 'no':'
# 输入 yes 按enter
[/home/anaconda3] >>> 
# 设置安装目录，直接enter就是默认的目录，显示在左边中括号那个
Unpacking payload ...  #出现就是就是在安装了
by running conda init? [yes|no] # 是否初始化conda，这里一定要输入yes 不要直接按enter，因为默认是no 然后就安装完成了
```

如果最后一步你输入了yes，那么打开~/.bashrc会看到这段配置



```bash
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/lucky/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/lucky/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/home/lucky/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/lucky/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

虽然conda设置了配置，但是并没有激活，需要激活一下

```bash
source ~/.bashrc
# 检查是否安装成功
conda -V
conda 4.10.3 # 出来这个就说明安装成功了
```

# 退出conda环境

```bash
conda deactivate
```


