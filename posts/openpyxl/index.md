# openpyxl基础操作



openpyxl操作Excel
<!--more-->

# openpyxl 写入Excel文件

由于`xlwt`只支持xls文件，所以对于Excel文件的处理，建议使用`openpyxl`进行操作

[参考博客](https://blog.csdn.net/weixin_41546513/article/details/109555832)

[官方文档](https://openpyxl.readthedocs.io/en/stable/)

## 创建文件

```python
def init_excel():
    wb = Workbook() # 创建Excel
    ws = wb.active # 激活worksheet
    lis = ['A', 'B', 'C', 'D', 'E', 'F'] 
    for i in lis:
        col = ws.column_dimensions[i]
        col.width = 13 # 设置列宽
    return wb, ws
```

在工作表中，列的索引是对应的英文字幕，行的索引是数字

## 读取Excel文件

```python
from pandas import read_excel

def readexcel(filename, row_end):
    try:
        data = read_excel(filename, 0)
        pass
    except Exception as e:
        print(str(e))
    else:
        res = data[0:row_end]
        return res
```

**其中data数据不包含标题**，0行表示第一条数据，一直到row_end-1行数据，即row_end条数据

## 写入数据

### 合并单元格

```python
worksheet.merge_cells(start_row=, end_row=, start_column=, end_column=)
```

其中行和列的索引都是从1开始

### 单元格赋值

```python
worksheet.cell(row, column).value = 'xxx'
```

### 添加一行值

```python
worksheet.append(value_list)
# 其中 value_list 为数组元素
```

## 样式设定

### 单元格填充颜色

```python
worksheet.cell(row, column).fill = PatternFill(patternType='solid', fgColor=Color(rgb='00D9EAF4'))
# fgColor   前景色
# bgColor   后景色
# 参数可选项
patternType = {'darkDown', 'darkUp', 'lightDown', 'darkGrid', 'lightVertical', 
               'solid', 'gray0625', 'darkHorizontal', 'lightGrid', 'lightTrellis', 
               'mediumGray', 'gray125', 'darkGray', 'lightGray', 'lightUp', 
               'lightHorizontal', 'darkTrellis', 'darkVertical'}
```

### 对齐样式

```python
worksheet.cell(row, column).alignment = Alignment(horizontal='center',vertical='center')
# 参数可选项
#horizontal = {'fill', 'distributed', 'centerContinuous', 'right',
              'justify', 'center', 'left', 'general'}

#vertical = {'distributed', 'justify', 'center', 'bottom', 'top'}
```

### 设置字体

```python
worksheet.cell(row, column).font = Font(name='宋体', size=10,  color=Color(rgb='00083866'), b=True)
# size: 字体大小
# b: bold  是否粗体
# i: italic  是否斜体
# name: 字体样式
```

## 示例代码

```python
def write_card(wt, dic, row_num):
    # content
    wt.merge_cells(start_row = row_num, end_row = row_num, start_column = 1, end_column = 6)
    wt.cell(row_num + 1, 1).value = '固定资产卡片'
    wt.merge_cells(start_row = row_num + 1, end_row = row_num + 1, start_column = 1, end_column = 6)
    wt.merge_cells(start_row = row_num + 2, end_row = row_num + 2, start_column = 1, end_column = 6)
    # TODO : use append method
    tmp_list1 = ['卡片编号', dic['card_no'], '资产名称', dic['name']]
    tmp_list2 = ['电商编号', dic['ec_code'], '分配人员', dic['person']]
    tmp_list3 = ['原值', dic['price'], '启用日期', dic['time'], '备注', dic['mark']]
    wt.append(tmp_list1)
    wt.append(tmp_list2)
    wt.append(tmp_list3)
    wt.merge_cells(start_row=row_num + 3, end_row=row_num + 3, start_column=4, end_column=6)
    wt.merge_cells(start_row=row_num + 4, end_row=row_num + 4, start_column=4, end_column=6)
    wt.merge_cells(start_row=row_num + 6, end_row=row_num + 6, start_column=1, end_column=6)

    # style
    wt.cell(row_num + 1, 1).fill = PatternFill(patternType='solid', fgColor=Color(rgb='00D9EAF4'))
    wt.cell(row_num + 1, 1).alignment = Alignment(horizontal='center',vertical='center')
    row1 = wt.row_dimensions[row_num + 1]
    row1.height = 18
    wt.cell(row_num + 6, 1).fill = PatternFill(patternType='solid', fgColor=Color(rgb='00D9EAF4'))
    wt.cell(row_num + 1, 1).font = Font(name='宋体', size=10,  color=Color(rgb='00083866'), b=True)
    for i in range(3, 6):
        for j in range(1, 5):
            wt.cell(row_num + i, j).font = Font(name='SimSun', size=9)
            wt.cell(row_num + i, j).alignment = Alignment(horizontal='left', vertical='center')
    wt.cell(row_num + 5, 5).font = Font(name='SimSun', size=9)
    wt.cell(row_num + 5, 5).alignment = Alignment(horizontal='left', vertical='center')
    wt.cell(row_num + 5, 6).font = Font(name='SimSun', size=9)
    wt.cell(row_num + 5, 6).alignment = Alignment(horizontal='left', vertical='center')
```




