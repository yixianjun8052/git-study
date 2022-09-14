tkinter，图形化 GUI。

```Python
# Tcl/Tk version
cmd: python -m tkinter

import tkinter as tk
# from tkinter import Tk,Entry,Button

# 开启窗体对象
root = tk.Tk()

# 设置装饰器。（tkinter会自动创建窗口装饰器，装饰器有以下几个部分：图标、标题、最大化、最小化和关闭按钮。在win10系统下，装饰器的最小宽度就是120，高度则不受影响。）
root.update()
root.winfo_width()
root.winfo_height()
root.geometry('300x300')	# 设置窗口大小，默认装饰器最小宽度为120
root.title('窗口标题')
root.iconbitmap('ico_path')	# 设置 ico 图标

# 创建控件对象
# 单行文本输入框：Entry
ent = tk.Entry(root)
ent.get()
ent.insert(index, string)	# index: 0 | index | 'end'
ent.delete(0, 'end')	# 清空
# 按钮
btn = tk.Button(root, text='按钮显示文本', command=func_name)	# 响应函数要事先定义。

# 多行文本：Text
text = tk.Text(root)
text.get(1.0, 'end') # 1表示第1行，0表示第1列，从首行首列开始读，直到结尾，即读取所有文本内容。
text.insert(index, string)

# 显示控件对象：默认布局下，控件按pack()顺序从上到下显示。
ent.pack()
btn.pack()
text.pack()

# 窗口主循环，必须放在最后
root.mainloop()
```

文件对话框
```PYTHON
>>> from tkinter.filedialog import askopenfilename,asksaveasfilename
>>> infileName = askopenfilename()
>>> outfileName = asksaveasfilename()
```