---
tags: os
---

操作 | 命令 os.xx
---- | ----
判断文件是否存在 | os.path.exists(path)
判断是否是一个文件 | os.path.isfile(path)
向操作系统下命令 | os.system(command)
查看当前路径 | os.getcwd()
切换目录 | os.chdir(path)
列出路径下的内容，不扫描子目录 | os.listdir()
列出路径下的内容，扫描子目录 | os.walk()
创建目录 | os.mkdir()
创建多级目录 | os.makedirs('t1/t2')
删除目录 | os.rmdir()
删除多级目录 | os.removedirs('t1/t2')
删除文件 | os.remove()
重命名 | os.rename()
递归重命名 | os.renames(old, new)
获取文件绝对路径 | os.path.abspath()
获取上级父目录 | os.path.dirname()

文件路径中，建议用正斜线 /。
```Python
>>> import os
>>> 
>>> os.path.exists("C:/Users/yi/Desktop/新建文本文档.txt")
True
>>> os.path.exists(r"C:\Users\yi\Desktop\新建文本文档.txt")
True
>>> os.path.exists("C:\\Users\\yi\\Desktop\\新建文本文档.txt")
True
>>> 
>>> os.system('start C:/Users/yi/Desktop/新建文本文档.txt')
0

# 向系统发送命令：打开文件 txt/pdf/mp3/mp4，windows中使用start
os.system('start D:/BaiduNetdiskDownload/第八套广播体操.mp3')

# 遍历目录下所有文件，包括子目录，批量重命名
for root, dirs, files in os.walk(path):
    for file in files:
        fpath = root.replace('\\', '/') + '/' + file
        print(fpath)

        # 批量重命名
        #new_name = pinyin.get(file, format='strip')
        #os.rename(root + '/' + file, root + '/' + new_name)
```