# 爬虫代理池的使用



requests-proxies
<!--more-->

# 报错解决

## requests.exceptions.SSLError: HTTPSConnectionPool

因为爬取的网站存在多次重定向导致报错

关闭证书验证`verify=False`

```python
response = requests.get('http://xxx.com/', headers = header, verify=False) 
```

[参考文章]([2020 python解决requests.exceptions.SSLError: HTTPSConnectionPool问题_ywj_486的博客-CSDN博客](https://blog.csdn.net/ywj_486/article/details/106479003)) 

## InsecureRequestWarning: Unverified HTTPS request is being made to host

```python
# 忽略requests证书警告
from requests.packages.urllib3.exceptions import InsecureRequestWarning
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)
```

# requests使用代理

```python
import requests

# 从ip代理池中获取ip和port
ip = proxydata['ip']
port = proxydata['port']
proxy = str(ip) + ':' + str(port)
proxies = {
   'http': 'http://' + proxy,
   'https': 'http://' + proxy
}
data = requests.get(get_url, proxies=proxies, verify=False, timeout=20)
data.close() # 注意关闭requests请求
resdata = data.text
```

# BeautifulSoup

## 初始化对象

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(data, "html.parser") # 解析器使用html.parse
```

## `find_all('tag')`方法

```python
# 返回第一个table标签中的内容
soup.find_all('table')[0]
# 返回table标签中 属性name为'imgShow'的 td标签内容
table_list = table.find_all('td', {"name": 'imgShow'})
```

`find('tag')`方法找到第一个符合的标签

`find_all('tag')`方法找到所有符合的标签,返回数组

## 获取标签的属性值

```python
for item in table_list:
    # 获取a标签中的title属性
    title = item.find('a').attrs['title']
    notice_title = str(title)
    # 获取当前td标签的id属性
    notice_time = str(item.attrs['id'])
```
