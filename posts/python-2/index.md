# Python自动化办公



读写Excel.写入Word
<!--more-->

# 写入Word(docx)

## 引入库

```python
from docx import Document # 文档
from docx.shared import Inches # 设置距离
from docx.shared import Cm, Pt # 设置单位
from docx.oxml.ns import qn  # 设置中文字体
from docx.enum.text import WD_PARAGRAPH_ALIGNMENT # 段落居中样式
from docx.enum.table import WD_CELL_VERTICAL_ALIGNMENT # 表格居中样式
```

## 创建word文件

```python
document = Document()
```

## 添加图片

```python
document.add_picture('dt_logo.png', width=Inches(4.165))
```

## 增加段落和文字

```python
p = document.add_paragraph()
p.alignment = WD_PARAGRAPH_ALIGNMENT.CENTER # 设置段落样式 居中
p4.paragraph_format.first_line_indent = Cm(0.74) # 设置首段缩进
a = p.add_run('电子商务平台交易服务费付款通知') # 段落中添加文字
a.font.name = u'仿宋_GB2312' # 设置字体
a._element.rPr.rFonts.set(qn('w:eastAsia'), u'仿宋_GB2312')
a.font.size = Pt(22) # 设置文字大小 单位:磅
a3.bold = True # 设置文字粗体
```

## 添加空行

```python
# 用了笨方法,添加一个没有内容的段落
blank1 = document.add_paragraph()
b1 = blank1.add_run(' ')
b1.font.name = u'仿宋_GB2312'
b1.font.size = Pt(22)
```

## 表格

```python
table = document.add_table(rows=2,  cols=2, style='Medium Shading 1 Accent 6') # 两行两列,选取官方表格样式
table.rows[0].height=Pt(25) # 第一行的高度
table.rows[1].height=Pt(50) # 第二行的高度
run = table.cell(0,0).paragraphs[0].add_run(u'收款单号') # 在(0,0)表格中添加文字
run.font.name = '仿宋' # 设置文字样式
run.font.size = Pt(11)
k1 = run._element
k1.rPr.rFonts.set(qn('w:eastAsia'), '仿宋')
table.cell(0,0).paragraphs[0].alignment = WD_PARAGRAPH_ALIGNMENT.CENTER # 水平居中
table.cell(0,0).vertical_alignment = WD_CELL_VERTICAL_ALIGNMENT.CENTER # 垂直居中
```

## 段落统一文字样式

```python
p5 = document.add_paragraph()
list_font = []
list_font.append(p5.add_run('支付方式：\n'))
tmp = p5.add_run('在线支付【推荐，实时结算】。')
tmp.bold = True
list_font.append(tmp)
list_font.append(p5.add_run('登录平台后，通过管理中心-支付平台-交易费收款单，弹出缴纳页面进行支付。\n联系人：大唐电子商务平台\n'))
list_font.append(p5.add_run('联系电话：400-888-6262\n\n\n\n'))
for i in range(len(list_font)):
	list_font[i].font.name = u'仿宋_GB2312'
	list_font[i]._element.rPr.rFonts.set(qn('w:eastAsia'), u'仿宋_GB2312')
	list_font[i].font.size = Pt(12)
```

# 读取Excel

# Pandas

Pandas 读取 Excel 是基于 xlrd 的封装

操作更方便,并且可使用Pandas各种数据处理方法

```python
from pandas import read_excel

try:
	#readbook = xlrd.open_workbook(r'info.xlsx')
	data = read_excel(r'info.xlsx', 1)
	pass
except Exception as e:
	print(str(e))
else:
    row_num = input("请输入数量:")
    row_num = int(row_num)
    res = data[0:row_num] # 返回前row_num行的数据
    return res

'''
返回DataFrame数据
'''
```

## xlrd

```python
import xlrd

# noinspection PyBroadException
try:
    readbook = xlrd.open_workbook(r'info.xlsx')
    pass
except Exception as e:
    #print("文件打开异常")
    print(str(e))
else:
    # 失败
    sheet = readbook.sheet_by_index(0) # 读取第一个sheet
    data_server = sheet.col_values(2, start_rowx=1, end_rowx=18) #读取服务端
    #server_data = get_interfaceList(get_ClassUrl.server.value, session)
    data_client = sheet.col_values(2, start_rowx=19, end_rowx=24) #读取客户端
    #client_data = get_interfaceList(get_ClassUrl.client.value, session)
    # 成功
    sheet = readbook.sheet_by_index(1)
    data_server2 = sheet.col_values(2, start_rowx=1, end_rowx=3)  # 读取服务端
    #server_data2 = get_interfaceList(get_ClassUrl.suc_server.value, session)
    data_client2 = sheet.col_values(2, start_rowx=4, end_rowx=7)  # 读取客户端
    #client_data2 = get_interfaceList(get_ClassUrl.suc_client.value, session)
```

## 读取数据

```python
sheet = readbook.sheet_by_index(0) # 读取第一个sheet
data_server = sheet.col_values(2, start_rowx=1, end_rowx=18) # 读取第三列 起始为第二行,终点为17行
'''
返回数组
'''
```






















