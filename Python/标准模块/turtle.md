turtle，海龟画图。

```Python
# 画布：
	setup()
# 画笔：
	penup()：pu()，抬笔，移动无轨迹
	pendown()：pd()，落笔
	pensize(width)：width(width)，粗细
	pencolor(color)：颜色，color: colorstring/r,g,b/(r,g,b)
# 运动：
	goto(x, y)
	fd(d)
	bk(d)
	circle(r, extent)：默认圆心在左侧，r为负则在右侧。extent为弧度。
# 方向：
.	seth(angle)：setheading()，绝对角度
	left((angle)
	right(angle)
# 写字：
	turtle.write('刘亦菲', font = ("Arial", 18, "normal"))

# 颜色填充：
	color()：设置画笔颜色和填充颜色。
	begin_fill()、end_fill()：begin 和 end 之间是画的图形。

home()：回到初始点和方向
clear()：清空画布
hideturtle()：隐藏画笔标识
done()：画布完成，停止画笔
```