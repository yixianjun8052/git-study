pyinstaller库：第三方库，可将py文件打包成对应平台的可执行文件

cmd中操作：
	pip install pyinstaller
	pyinstaller -i gujian.ico -F drawDigit.py
		-i：设置图标
		-F：在dist文件夹中只打包生成独立的一个可执行文件
		-D：默认值，除了exe外，还会在dist文件夹中生成很多依赖文件
		-w：不使用控制台，只对windows有用
		-p：导入路径

打包多个文件为一个可执行文件 .exe，使用 -F 打包主程序文件，-p 后接其他 .py 文件或目录，-p 可重复使用，再将其他依赖文件（如images文件夹）全部拖入到生成的dist文件夹中即可。
```cmd
pyinstaller -i pygame.ico -F -w alien_invasion.py -p alien.py -p bullet.py -p button.py -p game_functions.py -p game_stats.py -p scoreboard.py -p settings.py -p ship.py

# 也可将除主程序文件外的其他.py 文件，先放到一个目录，再使用 -p DIR 设置导入路径
pyinstaller -F -w alien_invasion.py -p py\
```
