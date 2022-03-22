# Selenium4.1语法更新


selenium4.1新版更新
<!--more-->

今天改点之前写的Selenium代码
但是发现都提示过期了,看了文档才知道selenium 4.1版本语法变了
记录一下新的语法

## Service对象

首先在引入`chromedriver`部分发生了改变,需要声明`Service`对象

```python
from selenium import webdriver
import re
import time
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service

service = Service(executable_path=r"E:\Webdriver\chromedriver")
option = webdriver.ChromeOptions()
option.binary_location = r"C:\Program Files\Google\Chrome\Application\chrome.exe"
option.add_argument("--lang=en-US")
option.add_experimental_option('w3c', True)
browser = webdriver.Chrome(service=service, options=option)
```

## 隐式等待

在看文档的过程发现了一个很棒的东西`implicitly_wait(10)`,叫隐式等待,参数是时间,单位为秒

例如设定时间为10s,但并不是一直等待10s,当脚本执行到某个元素定位时,如果元素存在,则继续执行,如果定位不到元素,则将以轮询的方式不断的判断元素是否存在,假设在第6秒定位到了元素,则继续执行,若直到超过设定的时间10s还没有定位到元素,则抛出异常

## 定位元素

### 通过`id`

```python
browser.find_element(By.ID, 'username')
```

### 通过`class_name`

```python
browser.find_element(By.CLASS_NAME, 'btn-submit')
```

### 通过`css`

```python
browser.find_element(By.CSS_SELECTOR, '.pagination-info')
```

### 通过`LINK_TEXT`

```python
browser.find_elements(By.LINK_TEXT, "推送法务系统")
```


