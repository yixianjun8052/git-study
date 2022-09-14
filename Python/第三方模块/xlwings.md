---
tags: excel读写
---

xlwings，是一个可以读写 Excel 数据表的第三方模块。支持NumPy中的数据结构。

操作步骤：进程app -> 工作簿book -> 工作表sheet -> 单元格范围range
```Python
pip install xlwings

import xlwings

# 打开程序
app = xlwings.App()
app.visible = False
app.quit()

# workbook 工作簿：App.books，返回 Book 对象。
wb = app.books.open(fpath)
wb = app.books.add()
wb.save(fpath)
wb.fullname
wb.name
wb.close()

# worksheet 工作表：Book.sheets，返回 Sheet 对象。
ws = wb.sheets.add()
ws = wb.sheets[0]
ws = wb.sheets['Sheet1']
ws.name
ws.clear()
ws.clear_content()

# range 单元格范围：Sheet.range(), 返回 Range 对象。
# 读写属性值：Range.value，不能写入集合set。
r = ws.range()
cont_list = r.value

ws.range('A1').value = '刘亦菲'
ws.range('A1').value = ['刘亦菲', 18, '王语嫣']
ws.range('A1:A3').value = ['刘亦菲', 18, '王语嫣']
ws.range('c3').value = [['刘亦菲', 18, '王语嫣'], ['刘德华', 40, '天若有情']]
ws.range('C3:E4').value = [['刘亦菲', 18, '王语嫣'], ['刘德华', 40, '天若有情']]

# 行列转置：原本写一行，转置为写一列
ws.range(’E2').option(transpose=True).value = [e1, e2, ...]

# 保存修改、关闭文件、程序
wb.save(fpath)
wb.close()
app.quit()
```

案例：读取 excel 数据
```Python
app = xlwings.App()
wb = app.books.open(r"C:\Users\yi\Desktop\degree.xlsx")
degree_list = wb.sheets['学位名单'].range('B3:C16').value
wb.close()
app.quit()
```