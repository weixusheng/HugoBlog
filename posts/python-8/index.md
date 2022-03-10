# Selenium执行自动化操作



Selenium Webdriver
<!--more-->


# 配置webdriver

## 创建webdriver

```python
option = webdriver.ChromeOptions()
option.binary_location = r"C:\Program Files\Google\Chrome\Application\chrome.exe"
# 设定英文启动
# option.add_argument("--lang=en-US");
browser = webdriver.Chrome(executable_path ="E:\Webdriver\chromedriver", options=option)
```

## 隐藏图片

加快运行速度

```python
prefs = {
            'profile.default_content_setting_values':{
                'images': 2,
            }
}
option.add_experimental_option('prefs', prefs)
```

## 设定窗口大小

```python
browser.set_window_size(1500,1000)
```

# 定位元素

## 访问网址

```python
# 访问网址
browser.get("http://tang.cdt-ec.com/cpu-portal-fe/login1.html")
```

## 填充元素

```python
username = browser.find_element_by_id('username').send_keys('***')
password = browser.find_element_by_id('password').send_keys('***')
```

## 通过class定位

```python
login_btn = browser.find_element_by_class_name('btn-submit')
login_btn.click()
time.sleep(5)
browser.get("http://bid.cdt-ec.com/dtdzzb/loginController.do?login&loginType=zb")
time.sleep(1)
browser.get("http://bid.cdt-ec.com/dtdzzb/tBContractInfoController.do?sendList")
time.sleep(5)
```

## 获得内部元素值

```python
count = browser.find_element_by_css_selector('.pagination-info').get_attribute('innerText') #总数量
```

## 正则表达式查找值

```python
num = re.search(r'Total (.*)Item', str(count), re.M | re.I).group(1)
print(num)
```

## 通过元素的Text属性查找

```python
lbtn_push = browser.find_elements_by_link_text("推送法务系统")
```

## find_elements和find_element

```python
elements返回所有匹配的数组
element只适用于查找单个存在的元素
```

# 循环执行操作

```python
num = int(num)
if num%10 != 0:
    shit = num/10+1
else:
    shit = num/10

for i in range(int(shit)-1):
    lbtn_push = browser.find_elements_by_link_text("推送法务系统")  # 推送按钮
    j = 0
    while j < len(lbtn_push):
        btn_push = browser.find_elements_by_link_text("推送法务系统")  # 推送按钮
        tmp_id = ""
        # noinspection PyBroadException
        try:
            order_id = browser.find_elements_by_class_name("datagrid-cell-c1-tenderno")  # 项目编号
            print(str(order_id[j].text))
            tmp_id = str(order_id[j].text)
            btn_push[j].click()
            click_ok = browser.find_element_by_link_text("确定")
            click_ok.click()
            time.sleep(4)
        except:
            pass
        window = ""
        # noinspection PyBroadException
        try:
            click_ok2 = browser.find_element_by_link_text("确定")
            window = browser.find_element_by_class_name("window-body").get_attribute('innerText')
        except:
            time.sleep(260)
            click_ok2 = browser.find_element_by_link_text("确定")
            window = browser.find_element_by_class_name("window-body").get_attribute('innerText')
        if str(window).find("成功") != -1:
            f = open('info.txt', 'a')
            strtime = time.strftime("%Y-%m-%d-%H:%M:%S", time.localtime())
            f.write('\n' + tmp_id + " " +strtime)
            f.close()
            j = j-1
        click_ok2.click()
        j = j+1
        time.sleep(2)
    next_btn = browser.find_element_by_class_name("pagination-next")
    next_btn.click()
    time.sleep(2)
```

