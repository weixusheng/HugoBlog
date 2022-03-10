# 协同过滤推荐系统



基于用户的(user-base)协同过滤推荐系统
<!--more-->

用于参加集团大数据竞赛
基于用户的(user-base)协同过滤推荐系统

# Pandas+openpyxl操作Excel

```python
from pandas import read_excel
from openpyxl import *

# 定义文件地址
addr = "res.xlsx"

# 加载excel文件
wb = load_workbook(addr)

# 加载sheet
ws = wb["Sheet1"]

# pandas读取excel文件 取第二个sheet
data = read_excel(r'1020.xlsx', 1)

for i in range(1, num-1):
    col1 = data.iloc[i]["loc"] # 读取第i+1行的 'loc'字段
    col2 = str(data.iloc[i]["fcode"])
    col3 = str(data.iloc[i]["scode"])
    col4 = str(data.iloc[i]["order"])
    col5 = data.iloc[i]["supplier"]
    res = col5.split(",", -1) # 对字符串进行分割,返回数组,参数为'-1',表示全部分割
    print(i)
    for j in range(0, len(res)):
        row = count
        # 在sheet中写入行
        ws.append([col1, col2, col3, col4, res[j]])
        count = count + 1
# 保存文件
wb.save(addr)
```

# 数据归一化处理

```python
import pandas as pd

# 归一化函数,对count列数值进行归一操作
def horizon(value):
    value["count"] = round((value["count"]/(value["count"].max() - value["count"].min()+1))*5,2)
    return value

list_code = pd.read_csv(r'code_list.csv')
data = pd.read_csv(r'../Mid_Data/train_data.csv')
for i in range(0, len(list_code)):
	# 对dataframe进行条件筛选
    data.loc[data["Scode"] == str(list_code.loc[i]["Scode"])] = horizon(data.loc[data["Scode"] == str(list_code.loc[i]["Scode"])])
data.to_csv(r'final_res.csv', index = False)
```

## Dataframe条件筛选

```python
# data.loc[条件语句]
data.loc[data["Scode"] == str(list_code.loc[i]["Scode"])]
```

# 归类数据返回字典

```python
def open_Loc(provice_name):
    file_data = open("../locANDname.csv",'r', encoding='UTF-8')
    tmpdata = {}
    for line in file_data.readlines()[1:]:
        line = line.strip().split(',')
        if not line[0] in tmpdata.keys():
            tmpdata[line[0]] = [line[1]]
        else:
            tmpdata[line[0]].append(line[1])
    return tmpdata[provice_name]
```

# 推荐系统算法

```python
from math import *
import Loc_list as loc_data

# 返回rating字典
def open_Rateing():
    file = open("one_res.csv", 'r', encoding='UTF-8')
    data = {}
    for tmpline in file.readlines()[1:]:
        tmpline = tmpline.strip().split(',')
        if not tmpline[0] in data.keys():
            data[tmpline[0]] = {tmpline[1]: tmpline[2]}
        else:
            data[tmpline[0]][tmpline[1]] = tmpline[2]
    return data
```

## 欧氏距离

计算的是两点间的直线距离

数值上的绝对差异

```python
def Euclidean(data, user1, user2):
    user1_data = data[user1]
    user2_data = data[user2]
    distance = 0
    for key in user1_data.keys():
        if key in user2_data.keys():
            distance += pow(float(user1_data[key]) - float(user2_data[key]), 2)
    return 1 / (1 + sqrt(distance))
```

## 皮尔逊相似系数

```python
def pearson_sim(data, user1, user2):
    user1_data = data[user1]
    user2_data = data[user2]
    common = {}
    for key in user1_data.keys():
        if key in user2_data.keys():
            common[key] = 1
    if len(common) == 0:
        return 0
    n = len(common)
    print(n, common)
    ##计算评分和
    sum1 = sum([float(user1_data[movie]) for movie in common])
    sum2 = sum([float(user2_data[movie]) for movie in common])
    ##计算评分平方和
    sum1Sq = sum([pow(float(user1_data[movie]), 2) for movie in common])
    sum2Sq = sum([pow(float(user2_data[movie]), 2) for movie in common])
    ##计算乘积和
    PSum = sum([float(user1_data[it]) * float(user2_data[it]) for it in common])
    ##计算相关系数
    num = PSum - (sum1 * sum2 / n)
    den = 0
    # noinspection PyBroadException
    try:
        shit1 = sum1Sq - pow(sum1, 2) / n
        shit2 = sum2Sq - pow(sum2, 2) / n
        cal_tmp = shit1 * shit2
        cal_tmp = round(cal_tmp, 3)
        den = sqrt(cal_tmp)
    except Exception as a:
        print(str(a))
    if den == 0:
        return 0
    r = num / den
    return r
```

## 杰卡德相似度

*Jaccard*（杰卡德）相似性系数主要用于计算符号度量或布尔值度量的样本间的相似度。

若样本间的特征属性由符号和布尔值标识，无法衡量差异具体值的大小，只能获得“是否相同*”*这样一种结果，而**Jaccard**系数关心的是样本间共同具有的特征。

```python
def jaccard(data, user1, user2):
    user1_data = data[user1]
    user2_data = data[user2]
    tmp = 0
    for key in user1_data.keys():
        if key in user2_data.keys():
            tmp = tmp+1
    fenmu = len(user1_data) + len(user2_data) - tmp  # 并集
    jaccard_coefficient = float(tmp / fenmu)  # 交集
    return jaccard_coefficient
```

## 返回相似度最高的10个

```python
def top10_simliar(data, AreaUser, userID):
    tmpres = []
    for userid in AreaUser:
        # 排除与自己计算相似度
        if not userid == userID:
            simliar = jaccard(data, userID, userid)
            #simliar = pearson_sim(data, userID, userid)
            tmpres.append((userid, simliar))
    #res.sort(key=lambda val: val[1])
    tmpres.sort(key=lambda val: val[1], reverse=True)
    return tmpres[:10]
```

## 主函数

```python
if __name__ == '__main__':
    rating_data = open_Rateing()
    area = "黑龙江省"
    area_list = loc_data[area]
    res = top10_simliar(rating_data, area_list, "江西远途电力技术服务有限公司")
    print(res)
```




