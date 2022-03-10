# 余弦相似度推荐系统



基于用户的(user-base)协同过滤推荐系统(cosine_similarity)
<!--more-->

# 使用sklearn库进行余弦计算

```python
import numpy as np
import pandas as pd
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics.pairwise import cosine_similarity
import Loc_list


df = pd.read_csv('../final_res.csv', sep=',', header=0)
# 供应商数量和商品数量
n_users = df.Name.unique().shape[0]
n_items = df.Scode.unique().shape[0]
```

## 数据编码

```python
# 进行数据编码
encoder_name = LabelEncoder()
user_id = encoder_name.fit_transform(df['Name'])
encoder_scode = LabelEncoder()
itm_id = encoder_scode.fit_transform(df['Scode'])
# 复制到新的DF中
tmp = df.copy()
# 编码增加到新的一列
tmp['user_id'] = user_id
tmp['item_id'] = itm_id
```

## lambda修改dataframe的一列数据

```python
tmp['Name'] = list(map(lambda x: x, user_id))
tmp['Scode'] = list(map(lambda x: x, itm_id))
```

## 查找该地区供应商

```python
area = "黑龙江省"
loc = Loc_list.open_Loc(area)
loc_supplier = tmp.loc[tmp['Name'].isin(loc)] # 数组范围内查询
```

## 创建训练集数据类型转换

```python
train_data = tmp
# 第一步是创建uesr-item矩阵，此处需创建训练和测试两个UI矩阵
train_data_matrix = np.zeros((n_users, n_items))
for line in train_data.itertuples(): # 遍历每一行,返回tuple元组
    #print(line)
    train_data_matrix[line[4]][line[5]] = line[3]
```

## 相似度计算

```python
user_similarity = cosine_similarity(train_data_matrix) # 返回 n_users * n_users 的矩阵
user_similarity = np.round(user_similarity, 3) # 小数点保留三位,方便计算和比较
```

## 供应商相似度计算

```python
res = encoder_name.transform(['江西远途电力技术服务有限公司']) #反向编码,键值获得编码值
dic = {}
for i in range(0, user_similarity[res].shape[1]):
    tmp_list = [i]
    dic[encoder_name.inverse_transform(tmp_list)[0]] = user_similarity[res][0][i]
res = sorted(dic.items(), key = lambda kv:(kv[1], kv[0]), reverse=True) # 对字典的value进行排序
print(res[1:10]) # 返回前十个相似度最高的供应商
```




