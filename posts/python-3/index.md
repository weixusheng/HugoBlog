# Python创建excel文件



xlwt写入Excel文件
<!--more-->

# 创建

```python
workbook = xlwt.Workbook()
worksheet = workbook.add_sheet('推送失败')
```

# 表格样式

## 行高

```python
worksheet.row(0).height_mismatch = True # 开启设置行高
worksheet.row(0).height = 20 * 30  # 20为基准数，40磅
```

## 表格文字居中

```python
style = xlwt.XFStyle()  # 创建一个样式对象，初始化样式
al = xlwt.Alignment() # al.horz = 0x02  # 设置水平居中
al.vert = 0x01  # 设置垂直居中
style.alignment = al

style2 = xlwt.XFStyle()
style2.alignment = al
pattern = xlwt.Pattern()  # Create the Pattern
# 设置表格底色(颜色标记)
pattern.pattern = xlwt.Pattern.SOLID_PATTERN  # May be: NO_PATTERN, SOLID_PATTERN, or 0x00 through 0x12
pattern.pattern_fore_colour = 5  # May be: 8 through 63. 0 = Black, 1 = White, 2 = Red, 3 = Green, 4 = Blue, 5 = Yellow, 6 = Magenta, 7 = Cyan, 16 = Maroon, 17 = Dark Green, 18 = Dark Blue, 19 = Dark Yellow , almost brown), 20 = Dark Magenta, 21 = Teal, 22 = Light Gray, 23 = Dark Gray, the list goes on...
style2.pattern = pattern
```

# 写入数据

```python
worksheet.write(0, 0, '法务系统机构名称', style2) # 第1行,第1列
worksheet.col(0).width = 10000 # 规定列宽
worksheet.write(0, 0, '法务系统机构名称', style2) # 第1行,第2列
worksheet.write(0, 1, '法务系统登陆账号', style2) # 第1行,第3列
worksheet.col(1).width = 7000
worksheet.write(0, 2, '电商平台机构名称', style2)
worksheet.col(2).width = 10000
worksheet.write(0, 3, '机构ID', style2)
worksheet.col(3).width = 7000
worksheet.write(0, 4, '人员名称', style2)
worksheet.col(4).width = 7000
worksheet.write(0, 5, '人员ID', style2)
worksheet.col(5).width = 7000
...
workbook.save('excel_name.xls') # 保存文件
```


