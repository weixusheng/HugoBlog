# 自动化接口监控



爬虫接口监控
<!--more-->

# 获取cookies

```python
import requests

# 返回所需要的登录牌照
def get_cookies():
    data = {'j_username': '***',
            'j_password': '***'}
    login_url = '***'
    session = requests.Session()
    session.keep_alive = False
    session.post(login_url, data)
    return session.cookies
```

# 处理json文件

```python
import json

def readJson(filename):
    filen = "./%s"%filename
    with open(filen, "r", encoding='utf-8') as f:
        loaded_dic = json.load(f)
        print(loaded_dic)
    return loaded_dic

def writeJson(filename, data):
    filen = "./%s" % filename
    with open(filen, "w", encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False)
        return True
```

# 得到监控数据

```python
import requests

headers = {
        'User-agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3112.113 Safari/537.36'
    }

def get_interfaceList(post_url,cook):
    """ 获得各个接口的报错 总数 和 接口唯一标识码
    目前仅支持服务端报错和客户端报错
    :param post_url: 请求网址
    :param cook: cookie登录令牌
    :return: list<dic>
    """
    result = requests.get(post_url, headers=headers, cookies=cook)
    result = result.json()
    return result["rows"]
```

# 建立监控url的枚举器

```python
from enum import Enum

class get_ClassUrl(Enum):
    server = "url1"
    client = "url2"
    suc_server = "url3"
    suc_client = "url4"
```

# 匹配异常接口数量

```python
def equal_error(info_data, online_data, resadd):
    count = len(online_data)
    for i in range(count):
        interface_name = online_data[i]['interfaceName']
        iserror = interface_name in info_data
        offline = int(info_data[interface_name])
        online = int(online_data[i]['errorNum'])
        if iserror:
            if offline != online:
                tmp = {"num": interface_name + "  失败数量异常",
                    "current": "当前数量:"+str(online),
                    "increase": "新增失败:"+str(online-offline)}
                info_data[interface_name] = online
                resadd.append(tmp)
        else:
            tmp = {"num": "失败接口 数量发生异常",
                   "current": "新增%s接口" % interface_name,
                   "increase": "新增数量%s" % str(online)}
            resadd.append(tmp)
            #print("当前 失败接口 数量发生异常, 新增%s接口 新增数量%s"%(interface_name, str(online)))
            info_data[interface_name] = online

# 校验成功
def equal_suc(info_data, online_data, resadd):
    count = len(online_data)
    for i in range(count):
        interface_name = online_data[i]['interfaceName']
        iserror = interface_name in info_data
        offline = int(info_data[interface_name])
        online = int(online_data[i]['successNum'])
        if iserror:
            if offline != online:
                tmp = {"num": interface_name +"  成功数量异常",
                    "current": "当前数量:" + str(online),
                    "increase": "新增成功:" + str(online - offline)}
                info_data[interface_name] = int(online)
                resadd.append(tmp)
        else:
            tmp = {"num": "成功接口 数量发生异常",
                   "current": "新增%s接口"%interface_name,
                   "increase": "新增数量%s"%str(online)}
            resadd.append(tmp)
            #print("当前 成功接口 数量发生异常, 新增%s接口 新增数量%s"%(interface_name, str(online)))
            info_data[interface_name] = online
```

# 主函数文件

```python
from GetCookies import get_cookies
from GetList import get_interfaceList
from UrlSelector import get_ClassUrl
import ExcData
import SendEmail

session = get_cookies()
json_data = ExcData.readJson("data.json")

# 读取json文件方式
server_data = get_interfaceList(get_ClassUrl.server.value, session)
client_data = get_interfaceList(get_ClassUrl.client.value, session)
server_data2 = get_interfaceList(get_ClassUrl.suc_server.value, session)
client_data2 = get_interfaceList(get_ClassUrl.suc_client.value, session)

# 验证
res = []
equal_error(json_data["fail_server"], server_data, res)
equal_error(json_data["fail_client"], client_data, res)
equal_suc(json_data["suc_server"], server_data2, res)
equal_suc(json_data["suc_client"], client_data2, res)
#print(json_data)
result = ExcData.writeJson("data.json", json_data)
if result:
    SendEmail.send_email("weixusheng@cweme.com", res)
else:
    print("System Error")
```


