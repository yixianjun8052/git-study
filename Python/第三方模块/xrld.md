xrld，读取 Excel 表格的第三方模块。表格数据必须为文本格式。

读取 excel 表格数据
excel = xlrd.open_workbook("./data/data1.xlsx")
sheet = excel.sheet_by_name("Sheet1")

获取行数、列数
sheet.nrows
sheet.ncols

获取行数据、列数据
sheet.row_values(0)
sheet.row_values(0, 1, 3)
sheet.col_values(1)
sheet.col_values(1, 1, 3))

获取单元格
sheet.row_values(2)\[3\]
```PYTHON
for row in range(sheet.nrows):
	if row == 0:
		continue
	for col in range(sheet.ncols):
		if col == 0:
			continue
		print(sheet.row_values(row)[col])
```


```python
import xlrd  


def read_excel(self, path, sheet_name):  
    sheet = xlrd.open_workbook(path).sheet_by_name(sheet_name)  
    row_num = sheet.nrows  
    col_num = sheet.ncols  
    data_list = []  
    key = sheet.row_values(0)  

    if row_num <= 1:  
        print("没数据")  
    else:  
        # 循环遍历表格中的每行数据  
        for i in range(row_num-1):  
            temp = sheet.row_values(i+1)  
            dic = {}  
            # 循环遍历每行数据中的每列数据，并匹配字典键值对  
            for j in range(col_num):  
                dic[key[j]] = temp[j]  
            # 每行数据匹配完键值对后，将字典作为元素添加到列表 data_list 中。  
            data_list.append(dic)  
    return data_list


#这是新添加的函数，用于处理和获取 Excel 文件中的测试数据
def read_excel(filename,index):
    xls = xlrd.open_workbook(filename)
    sheet = xls.sheet_by_index(index)
    #print(sheet.nrows)
    #print(sheet.ncols)
    dic={}
    for j in range(sheet.ncols):
        data=[]
        for i in range(sheet.nrows):
            data.append(sheet.row_values(i)[j])
        dic[j]=data
    return dic


import unittest
import requests

class Test(unittest.TestCase):
    def setUp(self) -> None:
        print("开始")

    def tearDown(self) -> None:
        print("结束")

    def test_001(self):
        url = "http://apis.juhe.cn/simpleWeather/query"
        paras = {"city": "北京", "key": "d6e7aae6149e367cfcc6ad8e04e945d5"}
        r = requests.post(url, data=paras)
        # print(r.json()["reason"])
        self.assertEqual(r.json()["reason"], "查询成功!")

    # 基于表格参数改写
    def test_003(self):
        url = "http://apis.juhe.cn/simpleWeather/query"
        for i in read_excel("./data/weatherdata.xlsx", "Sheet1"):
            paras = {"city": i["city"], "key": i["key"]}
            r = requests.post(url, data=paras)
            self.assertEqual(r.json()["error_code"], int(i["exp_error_code"]))

    # 加法运算
    def test_004(self):
        for i in read_excel("./data/data1.xlsx", "Sheet1"):
            self.assertEqual(int(i["a"]) + int(i["b"]), int(i["exp"]))


if __name__ == '__main__':
    unittest.main()
```