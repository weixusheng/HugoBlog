# Pyhton实现下载PDF



多线程下载PDF文件
<!--more-->

# 多线程下载PDF文件

```python
from Exc_xl import readexcel
import requests
from concurrent.futures import ThreadPoolExecutor, as_completed


data_num = 11681
data = readexcel('./res_data/GetReportInfo2.xlsx', data_num)
supplier_name = data['Orgname']

executor = ThreadPoolExecutor(max_workers=500) # 线程池500

def download_pdf(tmp_url, tmp_name):
    r = requests.get(tmp_url)
    fo = open(r'./FTP_data/' + str(tmp_name) + '.pdf', 'wb')
    fo.write(r.content)
    fo.close()
    print("success " + tmp_name)

threads = []
for index, url in enumerate(data['ReportUrl']):
    suburl = url
    name = supplier_name[index]
    args = [url, name]
    thread_spider = executor.submit(lambda p: download_pdf(*p), args)
    threads.append(thread_spider)

count = 1
for future in as_completed(threads):
    #data = future.result()
    print('当前进度: %s / %s' % (count, str(data_num)))
    count = count + 1
```
