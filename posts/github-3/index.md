# Git创建SSH公钥


## 填写git信息
```
git config --global user.name 你的英文名   #此英文名不需要跟GitHub账号保持一致
git config --global user.email 你的邮箱    #此邮箱不需要跟GitHub账号保持一致
```

## 生成本机ssh
```
ssh-keygen -t rsa -C "自己的邮箱地址"
```

## 查看ssh密钥
```
cat ~/.ssh/id_rsa.pub
```
