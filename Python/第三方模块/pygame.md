pygame，游戏库。

问题
```
游戏基本架构：初始化、实例对象、主循环（检测事件、对象移动、刷新屏幕）
游戏活动状态与非活动状态
窗口、对象、碰撞检测，设置、统计、节奏控制
外接矩形 rect

Bullet 继承 Sprite ?
Group 编组的作用 ？
pygame.mixer
sys.exit()、pygame.quit()
```

# 游戏基本框架

```Python
import pygame


def run_game():
	pygame.init()
	screen = pygame.display.set_mod(width_height_tuple)
	pygame.display.set_caption(title_str)
	
	while True:
		for event in pygame.event.get():
    		if event.type == pygame.QUIT:
    			sys.exit()
    			
    	screen.fill(bg_color_tuple)
    	
    	pygame.display.flip()
```

# 控制窗口

```Python
# 开启窗口：
screen = pygame.display.set_mod(width_height_tuple)

# 窗口尺寸控制：全屏、最小化、其它分辨率

# 关闭窗口：
import sys
for event in pygame.event.get():
    if event.type == pygame.QUIT:
    	sys.exit()

# 窗口标题：
pygame.display.set_caption(title_str)

# 窗口填充背景色：
screen.fill(bg_color_tuple)

# 屏幕刷新，显示最近屏幕更新
pygame.display.flip()
```

# 控制surface对象

```Python
# 基于 bmp 位图 图像对象
image = pygame.image.load('images/ship.bmp')
rect = image.get_rect()
screen.blit(image, rect)

# 基于 颜色填充 对象
rect = pygame.Rect(left, top, width, height)
pygame.draw.rect(screen, fill_color, rect)

# 基本 str 渲染成图像 对象
text = 'Play'  
text_color = (255, 0, 0)  
bg_color = (60, 60, 60)  
text_image = pygame.font.Font('C:\\Windows\\Fonts\\Arial.ttf', 88).render(text, True, text_color)  
text_image2 = pygame.font.SysFont(None, 48).render(text, True, text_color, bg_color)
```

控制外接矩形

矩形属性
宽高：width, w, height, h, 
中心点：center, centerx, centery, 
左上角顶点：x, y
上下：top, bottom, left, right, 
9点：topleft, midtop, topright, midleft, center, midright, bottomleft, midbottom, bottomright
```Python
# 根据对象获取对应的外接矩形 rect
image.get_rect()
   
# 解决坐标点赋值 float 数出现对象移动速度变化问题：使用中间浮点变量
self.centerx = float(self.rect.centerx)
self.centerx += self.ai_settings.ship_speed_factor
self.rect.centerx = self.centerx
```

判断矩形包含坐标

```Python
rect.collidepoint(x, y)

# 检查某个坐标是否在 rect 范围内。若要检查鼠标坐标，先获取鼠标坐标。  
rect.collidepoint(pygame.mouse.get_pos())
```

# 事件检测

```Python
# 检测键盘、鼠标事件
for event in pygame.event.get():
    if event.type == pygame.QUIT:
    	sys.exit()
    elif event.type == pygame.KEYDOWN:
        if event.key == pygame.K_q:
        	sys.exit()
        if event.key in [pygame.K_RIGHT, pygame.K_d]:
        	pass
    elif event.type == pygame.KEYUP:
    	check_keyup_events(event, ship)
	elif event.type == pygame.MOUSEBUTTONDOWN:
        mousex, mousey = pygame.mouse.get_pos()
		if button.button_rect.collidepoint(mousex, mousey) and event.button == 1 and not stats.game_active:
			stats.game_active = True
```

# 精灵编组

```Python
# 作用一：对所有编组内精灵共同管理行动
# 前提：精灵对象的类必须继承 pygame.sprite.Sprite
from pygame.sprite import Sprite

class Bullet(Sprite):

    def __init__(self, ai_settings, screen, ship):
        super().__init__()
        pass
        
# 建一个编组
from pygame.sprite import Group

bullets = Group()

# 将精灵加入编组
new_bullet = Bullet(ai_settings, screen, ship)
bullets.add(new_bullet)

# 编组精灵共同行动
bullets.update()

for bullet in bullets.sprites():
	bullet.draw_bullet()

# 计算编组长度
len(bullets)

# 删除消失的精灵 for bullet in bullets.sprites()
for bullet in bullets:
	if bullet.rect.bottom <= screen.get_rect().top:
		bullets.remove(bullet)
```

# 碰撞检测

```Python
# 编组碰撞检测：返回碰撞精灵的字典，value 是列表
collisions = pygame.sprite.groupcollide(bullets, aliens, True, True)


# 精灵编组碰撞：返回编组中的碰撞精灵
pygame.sprite.spritecollideany(ship, aliens)
```

# Alien Invasion