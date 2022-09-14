---
tags: matplotlib pyplot 数据可视化
---

数据可视化步骤：导入绘图模块 --> 构建坐标列表 --> 调用绘图方法 --> 调用显示方法。

导入绘图模块：
```Python
pip install matplotlib

from matplotlib.pyplot as plt
```

| 方法plt. | 说明 |
| --- | --- |
| scatter(x, y, s, c, marker) | 散点图 |
|| s, size, s=160, s=\[e1, e2\] |
|| c, color, c='red', c=\['r', 'g', 'b', 'y'\] |
|| marker, 设置点的形状, marker='\*' 五角星|
| plot(x, y) | 折线图。绘制双折线图 plot(x, y1, y2) |
| bar(x, height) | 柱状图 |
| pie(x, labels, autopct, explode, colors) | 饼状图 |
| rcParams\['font.sans-serif'\] | 设置字体。‘fangsong' 仿宋 |
| rcParams\['axes.unicode_minus'\] | 设置正确显示负号- |
| grid() | 设置网格线 |
| title() | 设置标题 |
| xticks(真实刻度, 刻度标签) | 将真实刻度与刻度标签关联，真实刻度不显示 |
| yticks() | 与xticks() 类似 |
| xlabel() | 设置x轴标签 |
| ylabel() | 设置y轴标签 |
| xlim() | xlim(0,200)，设置x轴范围 |
| ylim() | ylim(0,100)，设置y轴范围 |
| axis() | axis(\[0,200,0,100\])，设置x、y轴范围 |
| show() | 显示视图 |
| savefig(fpath) | 保存视图为图片png/jpg |

如果用数字列表作为横坐标，数据点间距会按数值自动调整。而字符串列表只能等距排列。

案例：日期与销量的拆线图
```Python
import matplotlib.pyplot as plt
from datetime import datetime

plt.rcParams['font.sans-serif'] = 'fangsong'
plt.rcParams['axes.unicode_minus'] = False

plt.grid()
plt.title('销量')

sales = [5, 22, 57, 78]
dates = ['21-03-01', '21-03-12', '21-03-16', '21-03-31']
days = []
for date in dates:
    delta = datetime.strptime(date, '%y-%m-%d') - datetime.strptime(dates[0], '%y-%m-%d')
    days.append(delta.days)
    
plt.xticks(days, dates)

plt.plot(days, sales)

plt.show()

```