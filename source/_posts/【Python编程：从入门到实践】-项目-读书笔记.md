---
title: 【Python编程：从入门到实践】-项目-读书笔记
date: 2025-08-21 09:49:13
tags:
  - python
  - note
---
# 【Python编程：从入门到实践】-项目-读书笔记

> 根据 [遗忘曲线](https://baike.baidu.com/item/%E9%81%97%E5%BF%98%E6%9B%B2%E7%BA%BF/7278665?fr=aladdin)：如果没有记录和回顾，6天后便会忘记75%的内容
>
>   读书笔记正是帮助你记录和回顾的工具，不必拘泥于形式，其核心是：记录、翻看、思考

| **书名** | Python编程：从入门到实践（第3版） |
| --- | --- |
| **原版书名** | Python Crash Course, 3rd Edition |
| **作者** | <font style="color:rgb(26, 28, 30);background-color:rgb(252, 252, 252);">[美] 埃里克·马瑟斯 (Eric Matthes)</font> |
| **译者** | 袁国忠 |
| **状态** | <font style="background:#DBF1B7;color:#2A4200">已读完</font>  |
| **简介** | **Python入门经典，用三大项目实战，带你从零快速上手。** |




## 项目一：外星人入侵 - 详细学习笔记
---

### **项目整体介绍**
**项目目标：**  
“外星人入侵”是一个经典的2D街机射击游戏。我们的最终目标是创建一个功能完整的游戏，在这个游戏中，玩家可以控制一艘飞船，射击不断向下入侵的外星人舰队，同时游戏会记录得分、最高分、玩家等级和剩余生命。

**核心技术：**  
我们将使用 **Pygame** 这个强大的Python第三方库。Pygame提供了一整套专门用于游戏开发的功能，包括创建窗口、绘制图形、处理用户输入（键盘、鼠标）、播放声音等。它让我们能专注于游戏的**逻辑**，而不用去处理底层的图形绘制细节。

**项目结构与学习路径：**  
这是一个结构化的项目，通过将不同的功能封装在不同的**类（Class）**和**模块（Module，即.py文件）**中来构建。这种方式是现代软件开发的标准实践，能让代码更清晰、更易于管理和扩展。我们将分三章完成：

+ **第12章：武装飞船** - 搭建游戏框架，创建玩家可控的飞船，并实现射击功能。
+ **第13章：外星人** - 创建外星人，组成舰队，让它们移动，并实现碰撞检测。
+ **第14章：记分** - 添加Play按钮，建立完整的记分系统，并让游戏难度递增。

---

### **第12章：武装飞船**
**本章核心目标：** 搭建游戏的基础框架，让一个可由玩家控制的飞船出现在屏幕上，并能发射子弹。

#### **1. 搭建游戏框架**
首先，我们需要创建一个窗口来作为游戏的舞台。

+ **创建主文件 **`alien_invasion.py`：这是游戏的启动文件和总控制器。
+ **创建**`AlienInvasion`**类**：

```python
# alien_invasion.py
import sys
import pygame

class AlienInvasion:
    """管理游戏资源和行为的类"""
    def __init__(self):
        """初始化游戏并创建游戏资源"""
        pygame.init() # 1. 初始化Pygame

        self.screen = pygame.display.set_mode((1200, 800)) # 2. 创建窗口
        pygame.display.set_caption("Alien Invasion")

    def run_game(self):
        """开始游戏的主循环"""
        while True: # 3. 游戏主循环
            # 侦听键盘和鼠标事件
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            pygame.display.flip() # 4. 刷新屏幕

if __name__ == '__main__':
    ai = AlienInvasion()
    ai.run_game()
```

**细节解析：**

    1. `pygame.init()`：初始化所有Pygame模块，是每个Pygame程序的必需步骤。
    2. `pygame.display.set_mode()`：创建一个窗口。它返回一个`surface`对象，这是屏幕上可以绘制图形的区域。
    3. `while True:`：游戏主循环，让游戏持续运行，不断检测输入和更新画面。
    4. `pygame.display.flip()`：刷新屏幕。在每次循环中，都会重新绘制屏幕，`flip`将最新的画面显示出来。

#### **2. 将设置模块化 (**`Settings`**类)**
为了不让代码变得混乱，我们把所有设置（如屏幕尺寸、颜色）集中管理。

+ **创建**`settings.py`**文件**：

```python
# settings.py
class Settings:
    """存储游戏中所有设置的类"""
    def __init__(self):
        """初始化游戏的设置"""
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (230, 230, 230) # RGB颜色：浅灰色
```

+ **在主程序中使用**：在`alien_invasion.py`中导入`Settings`类，并创建一个实例 `self.settings`。

#### **3. 创建飞船 (**`Ship`**类)**
我们创建一个单独的类来处理所有与飞船相关的逻辑。

+ **创建**`ship.py`**文件**：

```python
# ship.py
import pygame

class Ship:
    def __init__(self, ai_game):
        """初始化飞船并设置其初始位置"""
        self.screen = ai_game.screen
        self.screen_rect = ai_game.screen.get_rect() # 1. 获取屏幕的rect

        # 加载飞船图像并获取其外接矩形
        self.image = pygame.image.load('images/ship.bmp')
        self.rect = self.image.get_rect() # 2. 获取飞船的rect

        # 每艘新飞船都放在屏幕底部的中央
        self.rect.midbottom = self.screen_rect.midbottom # 3. 定位

    def blitme(self):
        """在指定位置绘制飞船"""
        self.screen.blit(self.image, self.rect) # 4. 绘制
```

**细节解析：**

    1. `get_rect()`：获取一个`surface`（无论是屏幕还是图像）的**矩形区域（rect）**。
    2. `rect`对象：你可以把它想象成一个围绕图像的隐形框，通过操作它的属性（如`.midbottom`）来轻松定位图像。
    3. `midbottom`：是`rect`的一个属性，代表矩形底边的中心点。将飞船的`midbottom`设置为屏幕的`midbottom`，就能让飞船居中于屏幕底部。
    4. `blit()`：是“绘制”的意思，将一个图像(`self.image`)画在另一个`surface`(`self.screen`)上由`self.rect`指定的位置。

#### **4. 实现飞船移动**
+ **使用移动标志**：在`Ship`类的`__init__`中添加标志 `self.moving_right = False` 和 `self.moving_left = False`。
+ **更新**`_check_events()`**方法**：

```python
# alien_invasion.py (在_check_events方法中)
elif event.type == pygame.KEYDOWN: # 按键按下
    if event.key == pygame.K_RIGHT:
        self.ship.moving_right = True
elif event.type == pygame.KEYUP: # 按键松开
    if event.key == pygame.K_RIGHT:
        self.ship.moving_right = False
```

+ **创建**`Ship`**类的**`update()`**方法**：

```python
# ship.py
def update(self):
    """根据移动标志调整飞船的位置"""
    if self.moving_right:
        self.rect.x += 1 # 暂时移动1个像素
```

+ **在主循环中调用**：在`run_game()`的`while`循环里调用`self.ship.update()`，这样每一帧都会检查标志并移动飞船。

#### **5. 实现射击功能**
1. **创建**`Bullet`**类 (**`bullet.py`**)**：

```python
# bullet.py
import pygame
from pygame.sprite import Sprite

class Bullet(Sprite):
    def __init__(self, ai_game):
        super().__init__() # 必须调用父类的__init__
        self.screen = ai_game.screen
        self.settings = ai_game.settings
        self.color = self.settings.bullet_color

        # 在(0, 0)处创建一个表示子弹的矩形，然后设置正确的位置
        self.rect = pygame.Rect(0, 0, self.settings.bullet_width,
            self.settings.bullet_height)
        self.rect.midtop = ai_game.ship.rect.midtop # 从飞船顶部发出

        # 存储用浮点数表示的子弹位置
        self.y = float(self.rect.y)

    def update(self):
        """向上移动子弹"""
        self.y -= self.settings.bullet_speed # y坐标减小
        self.rect.y = self.y

    def draw_bullet(self):
        """在屏幕上绘制子弹"""
        pygame.draw.rect(self.screen, self.color, self.rect)
```

    - 这个类需要继承`pygame.sprite.Sprite`，这让我们能使用**编组（Group）**来统一管理所有子弹。
2. **管理子弹**：
    - 在`AlienInvasion`中创建一个编组：`self.bullets = pygame.sprite.Group()`。
    - 当玩家按下空格键时 (`K_SPACE`)，创建一个`Bullet`实例并将其添加到`self.bullets`编组中。
    - 在主循环中，调用`self.bullets.update()`来移动所有子弹。
    - 在`_update_screen()`中，遍历`self.bullets`编组，并对每个子弹调用`draw_bullet()`方法。
    - **删除消失的子弹**：遍历`self.bullets.copy()`，检查子弹是否飞出屏幕顶部(`bullet.rect.bottom <= 0`)，如果是，则从`self.bullets`中移除它。

---

### **第13章：外星人**
**本章核心目标：** 在屏幕上创建一群外星人，让它们能够作为一个整体移动，并实现子弹与外星人、外星人与飞船的碰撞检测。

#### **1. 创建外星人**
+ **创建**`Alien`**类 (**`alien.py`**)**：这个类的结构与`Ship`类非常相似，也需要加载图像、获取`rect`。

```python
# alien.py
class Alien(Sprite):
    def __init__(self, ai_game):
        # ...
        self.image = pygame.image.load('images/alien.bmp')
        self.rect = self.image.get_rect()
        
        # 每个外星人最初都在屏幕左上角附近
        self.rect.x = self.rect.width
        self.rect.y = self.rect.height
        # ...
```

#### **2. 创建外星舰队**
+ **创建**`_create_fleet()`**方法**：这个方法负责计算并创建整支舰队。

```python
# alien_invasion.py (_create_fleet方法)
def _create_fleet(self):
    """创建外星舰队"""
    alien = Alien(self)
    alien_width, alien_height = alien.rect.size
    # ... 计算一行可容纳多少外星人，以及可容纳多少行 ...

    for row_number in range(number_rows):
        for alien_number in range(number_aliens_x):
            self._create_alien(alien_number, row_number)
```

+ **使用嵌套循环**：通过两层`for`循环，我们可以按行和列来创建外星人，并精确计算每个外星人的位置。

#### **3. 让舰队移动**
1. **添加设置**：在`settings.py`中添加`alien_speed`、`fleet_drop_speed`和`fleet_direction`（`1`表示向右，`-1`表示向左）。
2. `Alien`**类的**`update()`**方法**：让每个外星人根据`fleet_direction`来移动。

```python
# alien.py
def update(self):
    """向右或向左移动外星人"""
    self.x += (self.settings.alien_speed *
                    self.settings.fleet_direction)
    self.rect.x = self.x
```

3. **边缘检测**：在`Alien`类中添加`check_edges()`方法，检查外星人是否碰到屏幕边缘。
4. **在**`AlienInvasion`**中控制舰队移动**：
    - 创建`_update_aliens()`方法，它会调用`self.aliens.update()`。
    - 在此方法中，调用`_check_fleet_edges()`。
    - `_check_fleet_edges()`遍历舰队，如果发现有外星人碰到边缘，就调用`_change_fleet_direction()`。
    - `_change_fleet_direction()`会让整个舰队下移，并反转移动方向。

#### **4. 碰撞检测与游戏结束**
1. **子弹与外星人碰撞**：

```python
# alien_invasion.py (_update_bullets方法)
collisions = pygame.sprite.groupcollide(
        self.bullets, self.aliens, True, True)
```

    - **核心函数**: `pygame.sprite.groupcollide()`
    - 这行代码会检查`bullets`编组和`aliens`编组之间是否有成员碰撞。后两个`True`参数表示**碰撞后立即销毁**子弹和外星人。
    - 当所有外星人被消灭后（即`self.aliens`为空），清空子弹并重新创建舰队。
2. **外星人与飞船碰撞**：

```python
# alien_invasion.py (_update_aliens方法)
if pygame.sprite.spritecollideany(self.ship, self.aliens):
    self._ship_hit()
```

    - **创建**`GameStats`**类 (**`game_stats.py`**)**：用于跟踪游戏统计数据，如剩余飞船数`ships_left`。
    - **核心函数**: `pygame.sprite.spritecollideany()`
    - `_ship_hit()`**方法**：
        1. 检查`ships_left`是否大于0。
        2. 如果大于0，则将`ships_left`减1，清空屏幕，重置飞船和外星人，并暂停0.5秒。
        3. 如果等于0，则将游戏状态`game_active`设为`False`，游戏结束。

---

### **第14章：记分**
**本章核心目标：** 完善游戏体验，添加Play按钮、记分系统和难度递增机制。

#### **14.1 添加Play按钮**
1. `Button`**类 (**`button.py`**)**：创建一个自定义的按钮类，因为它不是`pygame`内置的。
    - 它会创建一个矩形，并使用`pygame.font`模块将文本渲染成图像，然后将文本图像绘制在矩形中央。
2. **管理游戏状态**：
    - 在`AlienInvasion`的`__init__`中，将`game_active`标志**初始设为**`False`。
    - 在`_update_screen()`中，**仅当**`game_active`**为**`False`**时**才绘制Play按钮。
3. **响应点击**：
    - 在`_check_events()`中检测`MOUSEBUTTONDOWN`事件。
    - 使用`button.rect.collidepoint(mouse_pos)`判断点击位置是否在按钮内。
    - 如果点击了按钮且游戏未开始，则**重置游戏**（`stats.reset_stats()`）、将`game_active`设为`True`，并隐藏鼠标光标。

#### **14.2 提高难度**
+ **在**`Settings`**中分离设置**：将设置分为**静态设置**和**动态设置**。
+ `increase_speed()`**方法**：在`settings.py`中创建一个方法，当调用它时，飞船、子弹和外星人的速度都会乘以一个加速因子（如`1.1`）。
+ **调用时机**：当整支外星舰队被消灭后，在创建新舰队之前，调用`settings.increase_speed()`。

#### **14.3 记分系统**
1. `Scoreboard`**类 (**`scoreboard.py`**)**：创建一个类来处理所有得分信息的显示。

```python
# scoreboard.py
class Scoreboard:
    def __init__(self, ai_game):
        # ... 初始化字体、颜色等 ...
        self.stats = ai_game.stats
        # ...
    
    def prep_score(self):
        """将得分渲染为图像"""
        score_str = str(self.stats.score)
        self.score_image = self.font.render(score_str, True,
                self.text_color, self.settings.bg_color)
        # ... 定位得分图像 ...

    def show_score(self):
        """在屏幕上显示得分"""
        self.screen.blit(self.score_image, self.score_rect)
```

2. **更新和显示**：
    - **得分**：当子弹击中外星人时，增加`stats.score`的值，然后立即调用`sb.prep_score()`来更新显示的图像。
    - **最高分**：在`GameStats`中添加`high_score`属性。每次得分增加后，都检查是否超过了最高分，如果是则更新`high_score`和对应的图像。
    - **等级**：在`GameStats`中添加`level`属性。每当消灭一波外星人，就将等级加1并更新图像。
    - **剩余飞船**：不再用数字显示，而是在屏幕左上角绘制相应数量的小飞船图标。这需要让`Ship`类也继承`Sprite`，并在`Scoreboard`中创建一个飞船编组来管理这些图标。

通过这三章的学习，一个完整、动态且具有挑战性的“外星人入侵”游戏就完成了。这个过程全面覆盖了从项目搭建、面向对象编程、事件处理到游戏逻辑实现的方方面面，是巩固和应用Python基础知识的绝佳实践。

## 项目二：数据可视化
---

### **项目整体介绍**
**项目目标：**  
本项目旨在教会我们如何使用Python来处理数据并将其以图形化的方式呈现出来。数据可视化是**数据分析**的核心环节，它能帮助我们直观地发现数据中的模式、趋势和异常点，将枯燥的数字转换成一目了然的图表。

**核心技术：**  
我们将学习使用两个强大的Python数据可视化库：

1. **Matplotlib**：一个非常流行且功能全面的绘图库，被誉为Python数据可视化领域的“元老”。它能创建高质量的静态图表，如图线图、散点图等。
2. **Plotly**：一个现代化的交互式绘图库。它生成的图表可以在网页浏览器中显示，用户可以通过鼠标悬停、缩放等方式与图表进行互动，非常适合制作动态和交互式的数据报告。

**项目结构与学习路径：**  
我们将通过三个循序渐进的章节来完成这个项目：

+ **第15章：生成数据** - 学习Matplotlib和Plotly的基本用法，通过绘制简单的图表和模拟随机游走来生成和可视化数据。
+ **第16章：下载数据** - 学习如何处理真实世界的数据文件，特别是常见的**CSV格式**。我们将下载并可视化气象数据。
+ **第17章：使用API** - 学习如何通过**API（应用程序编程接口）**从网络服务（如GitHub）上自动获取实时数据，并进行可视化。

---

### **第15章：生成数据**
**本章核心目标：** 掌握Matplotlib和Plotly的基本绘图流程，学会生成简单的数据集并将其可视化。

#### **1. 安装Matplotlib和Plotly**
+ **安装命令**：在终端中使用`pip`来安装这两个库。

```bash
python -m pip install --user matplotlib
python -m pip install --user plotly pandas
```    *   **注意**：Plotly Express通常需要`pandas`库的支持，所以我们一并安装。
```

#### **2. 使用Matplotlib绘制简单的折线图**
1. **基本绘图流程**：

```python
# mpl_squares.py
import matplotlib.pyplot as plt

squares = [1, 4, 9, 16, 25]

fig, ax = plt.subplots() # 1. 创建图表和绘图区域
ax.plot(squares)         # 2. 绘制数据

plt.show()               # 3. 显示图表
```

**细节解析：**

    1. `import matplotlib.pyplot as plt`：这是使用`pyplot`模块的约定俗成的方式。
    2. `plt.subplots()`：创建一个包含图表（`fig`）和单个绘图区域（`ax`）的窗口。`ax`是我们用来绘制图形的主要对象。
    3. `ax.plot()`：根据提供的数据列表绘制折线图。
    4. `plt.show()`：打开Matplotlib的查看器并显示绘制好的图表。
2. **定制图表**：

```python
# mpl_squares.py (修改后)
import matplotlib.pyplot as plt

input_values = [1, 2, 3, 4, 5]
squares = [1, 4, 9, 16, 25]

plt.style.use('seaborn') # 1. 使用预设样式
fig, ax = plt.subplots()
ax.plot(input_values, squares, linewidth=3) # 2. 提供x,y值，设置线宽

# 3. 设置图表标题和坐标轴标签
ax.set_title("Square Numbers", fontsize=24)
ax.set_xlabel("Value", fontsize=14)
ax.set_ylabel("Square of Value", fontsize=14)

# 4. 设置刻度标记的大小
ax.tick_params(labelsize=14)

plt.show()
```

**细节解析：**

    - 我们可以添加标题、坐标轴标签，并设置线条样式。
    1. `plt.style.use()`：可以使用Matplotlib内置的样式（如`'seaborn'`）来美化图表。
    2. `ax.plot(x, y)`：同时提供x轴和y轴的数据，可以确保图表被正确地绘制。
    3. `ax.set_title()`, `ax.set_xlabel()`, `ax.set_ylabel()`：用于设置图表的各种标签。
    4. `ax.tick_params()`：用于定制坐标轴刻度的外观。

#### **3. 使用Matplotlib绘制散点图 (**`scatter`**)**
+ **功能**：用于绘制单个或一系列的数据点。
+ **自动计算数据**：我们可以用`range()`和列表推导式来自动生成大量的数据点。

```python
# scatter_squares.py
import matplotlib.pyplot as plt

x_values = range(1, 1001)
y_values = [x**2 for x in x_values]

plt.style.use('seaborn')
fig, ax = plt.subplots()
# 使用颜色映射 (colormap)
ax.scatter(x_values, y_values, c=y_values, cmap=plt.cm.Blues, s=10)

# ... 设置标题和标签 ...

# 设置每个坐标轴的取值范围
ax.axis([0, 1100, 0, 1100000])

plt.show()
```

**细节解析：**

    - `ax.scatter()`：绘制散点图。
    - `s=10`：设置数据点的大小。
    - `c=y_values, cmap=plt.cm.Blues`：这是一个高级用法，`c`参数用于指定颜色，当它被设置为一个数值列表时，`cmap`（颜色映射）会根据这些数值的大小将点映射成不同的颜色（这里是从浅蓝到深蓝）。

#### **4. 模拟随机游走**
+ `RandomWalk`** 类**：创建一个类来模拟随机游走的过程。

```python
# random_walk.py
from random import choice

class RandomWalk:
    def __init__(self, num_points=5000):
        self.num_points = num_points
        self.x_values = [0]
        self.y_values = [0]

    def fill_walk(self):
        while len(self.x_values) < self.num_points:
            x_direction = choice([1, -1])
            x_distance = choice([0, 1, 2, 3, 4])
            x_step = x_direction * x_distance
            
            # ... 对y_step进行类似计算 ...
            
            # 拒绝原地踏步
            if x_step == 0 and y_step == 0:
                continue

            # 计算下一个点的x和y值
            x = self.x_values[-1] + x_step
            y = self.y_values[-1] + y_step
            
            self.x_values.append(x)
            self.y_values.append(y)
```

+ **可视化随机游走**：

```python
# rw_visual.py
import matplotlib.pyplot as plt
from random_walk import RandomWalk

while True: # 不断生成新的随机游走图
    rw = RandomWalk()
    rw.fill_walk()

    plt.style.use('classic')
    fig, ax = plt.subplots()
    point_numbers = range(rw.num_points)
    ax.scatter(rw.x_values, rw.y_values, c=point_numbers, cmap=plt.cm.Blues,
        edgecolors='none', s=15)
    
    # 突出起点和终点
    ax.scatter(0, 0, c='green', edgecolors='none', s=100)
    ax.scatter(rw.x_values[-1], rw.y_values[-1], c='red', edgecolors='none', s=100)
    
    # 隐藏坐标轴
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)
    
    plt.show()

    keep_running = input("Make another walk? (y/n): ")
    if keep_running == 'n':
        break
```

#### **5. 使用Plotly模拟掷骰子**
1. `Die`** 类 (**`die.py`**)**：创建一个简单的类来模拟骰子。

```python
# die.py
from random import randint

class Die:
    def __init__(self, num_sides=6):
        self.num_sides = num_sides

    def roll(self):
        return randint(1, self.num_sides)
```

2. **分析结果并可视化**：

```python
# die_visual.py
from die import Die
import plotly.express as px

die = Die() # 创建一个6面骰子

# 掷骰子，并将结果存储在列表中
results = [die.roll() for roll_num in range(1000)]

# 分析结果
poss_results = range(1, die.num_sides + 1)
frequencies = [results.count(value) for value in poss_results]

# 可视化结果
title = "Results of Rolling One D6 1,000 Times"
labels = {'x': 'Result', 'y': 'Frequency of Result'}
fig = px.bar(x=poss_results, y=frequencies, title=title, labels=labels)
fig.show()
```

**细节解析：**

    - 掷多次骰子，统计每个点数出现的频率。
    - `import plotly.express as px`：这是使用Plotly Express的约定。
    - `px.bar()`：创建一个**条形图（直方图）**，非常适合显示频率分布。
    - `fig.show()`：在浏览器中打开并显示交互式图表。
3. **定制Plotly图表**：可以为图表添加标题和标签，甚至更进一步地定制布局。

```python
# ... 在fig = px.bar(...)之后添加
fig.update_layout(xaxis_dtick=1) # 确保每个x轴刻度都显示
```

---

### **第16章：下载数据**
**本章核心目标：** 学习处理存储在文件中的真实世界数据集，重点是**CSV（逗号分隔值）文件**，并使用Matplotlib将其可视化。

#### **1. CSV文件格式**
+ **概念**：一种非常常见的、用纯文本存储表格数据的文件格式。每行代表一条记录，记录中的字段用逗号分隔。

#### **2. 解析CSV文件头**
+ `csv`**模块**：Python标准库中用于处理CSV文件的模块。

```python
# sitka_highs.py
from pathlib import Path
import csv

path = Path('weather_data/sitka_weather_07-2021_simple.csv')
lines = path.read_text().splitlines()

reader = csv.reader(lines) # 1. 创建reader对象
header_row = next(reader)  # 2. 读取文件头

for index, column_header in enumerate(header_row):
    print(index, column_header)
```

**细节解析：**

    1. `csv.reader()`：接收一个文件对象（或行列表），并创建一个`reader`对象，可以方便地遍历CSV的每一行。
    2. `next()`：当用于`reader`对象时，它会返回文件中的下一行。第一次调用时返回的就是文件头。

#### **3. 提取并读取数据**
+ **遍历数据**：在读取文件头之后，继续对`reader`对象进行`for`循环，就可以遍历文件的其余数据行。
+ **数据转换**：从CSV文件中读取的数据都是**字符串**，如果需要进行计算，必须用`int()`或`float()`转换为数字。

```python
# sitka_highs.py
highs = []
for row in reader:
    high = int(row[4]) # 假设最高温在第5列（索引为4）
    highs.append(high)
```

#### **4. 处理日期数据 (**`datetime`**模块)**
+ **功能**：Python标准库中用于处理日期和时间的模块。
+ `datetime.strptime()`：将**字符串**格式的日期转换为Python能够理解的`datetime`**对象**。

```python
from datetime import datetime

# 格式代码 '%Y-%m-%d' 告诉Python如何解析日期字符串'2021-07-01'
first_date = datetime.strptime('2021-07-01', '%Y-%m-%d')
```

+ **在图表中使用日期**：将日期字符串转换为`datetime`对象后，就可以将它们作为x轴数据传递给`plot()`，Matplotlib会自动处理日期的显示。
    - `fig.autofmt_xdate()`：自动格式化x轴的日期标签，使其倾斜以避免重叠。

#### **5. 错误检查**
+ **问题**：真实世界的数据集常常是不完整的，比如某一天的数据缺失。
+ **解决方法**：使用**`try-except-else`代码块**。在尝试转换数据（如`int()`）时，如果遇到空字符串或其他无效值，会引发`ValueError`。我们可以捕获这个异常，并打印一条错误消息，然后继续处理下一行数据。

```python
for row in reader:
    try:
        high = int(row[3])
        low = int(row[4])
    except ValueError:
        print(f"Missing data for {current_date}")
    else:
        highs.append(high)
        lows.append(low)
```

---

### **第17章：使用API**
**本章核心目标：** 学习如何通过**API（应用程序编程接口）**自动从网络服务获取实时数据，并使用Plotly进行可视化。

#### **1. 什么是API**
+ **概念**：API是网站提供给程序用来交互的一套规则和工具。程序可以通过访问特定的URL（称为**API调用**）来请求数据，网站会以易于处理的格式（通常是**JSON**）返回数据。

#### **2. 使用Requests库**
+ `requests`：一个非常流行的第三方库，用于发送HTTP请求（即进行API调用）。
+ **安装**：`python -m pip install --user requests`
+ **进行API调用**：

```python
# python_repos.py
import requests

url = 'https://api.github.com/search/repositories?q=language:python+sort:stars'
headers = {'Accept': 'application/vnd.github.v3+json'} # 1. 指定API版本
r = requests.get(url, headers=headers) # 2. 发送GET请求
print(f"Status code: {r.status_code}") # 3. 检查状态码
```

**细节解析：**

    1. `headers`：HTTP标头，可以向API提供额外信息，这里我们指定使用GitHub API的第3版。
    2. `requests.get()`：发送一个GET请求到指定的URL。
    3. `r.status_code`：响应对象`r`的状态码。**状态码200表示请求成功**。

#### **3. 处理API响应**
+ **JSON格式**：API返回的数据通常是JSON格式。
+ **解析JSON**：`requests`库内置了JSON解码器，可以轻松地将响应转换为Python字典。

```python
response_dict = r.json()
```

+ **探索数据**：转换成字典后，我们就可以通过键来访问数据了，就像处理普通字典一样。

```python
print(f"Total repositories: {response_dict['total_count']}")

repo_dicts = response_dict['items'] # 获取仓库信息列表

# 遍历列表，处理每个仓库的信息
for repo_dict in repo_dicts:
    print(f"\nName: {repo_dict['name']}")
    print(f"Owner: {repo_dict['owner']['login']}")
    # ...
```

#### **4. 使用Plotly可视化API返回的数据**
+ **流程**：
    1. 执行API调用并获取数据。
    2. 遍历返回的数据，将需要可视化的信息（如仓库名、星标数、悬停文本等）提取并存储到列表中。
    3. 使用`plotly.express`（如`px.bar()`）创建图表，将这些列表作为数据源。
+ **添加交互功能**：

```python
# ... 在循环中 ...
repo_name = repo_dict['name']
repo_url = repo_dict['html_url']
repo_link = f"<a href='{repo_url}'>{repo_name}</a>"
repo_links.append(repo_link)

# ... 在绘图时 ...
fig = px.bar(x=repo_links, y=stars, ...)
```

    - **悬停文本 (Hover Text)**：在`px.bar()`中设置`hover_name`参数，可以在鼠标悬停在条形上时显示更详细的信息。
    - **可点击链接**：可以创建包含HTML `<a>` 标签的字符串列表，并将其作为x轴数据。这样图表中的标签就变成了可点击的链接。

---

#### **项目总结**
项目二带领我进入了激动人心的数据科学领域。从**第15章**学会使用Matplotlib和Plotly生成基本图表，到**第16章**处理真实的CSV气象数据，再到**第17章**通过API获取实时的网络数据，我逐步掌握了数据可视化的完整流程。我不仅学会了如何画图，更重要的是学会了**如何准备数据、处理数据中的不完美（如缺失值）、以及如何通过定制图表来更清晰地讲述数据背后的故事**。这个项目让我深刻体会到，编程不仅仅是创建应用，更是一种探索和理解世界的强大工具。

## 项目三：Web应用程序
---

### **项目整体介绍**
**项目目标：**  
本项目的目标是创建一个名为“学习笔记”（Learning Log）的**Web应用程序**。用户可以通过网页浏览器访问这个应用，注册账户，然后创建自己学习的主题，并为每个主题添加学习笔记条目。这个项目将教会我们构建一个功能完整的、数据驱动的、支持多用户的动态网站。

**核心技术：**  
我们将使用 **Django**，这是一个非常流行且功能强大的Python **Web框架**。框架是一套工具和规则的集合，它极大地简化了Web应用的开发过程。Django为我们处理了很多复杂的底层工作，比如：

+ **URL路由**：将用户请求的网址映射到正确的处理逻辑。
+ **数据库交互**：通过**模型（Models）**来定义数据结构，Django会自动将其转换为数据库表，并提供简单的API来操作数据。
+ **用户认证**：内置了完整的用户注册、登录、注销系统。
+ **模板系统**：将后端逻辑与前端HTML页面分离，让我们可以动态生成网页内容。
+ **安全性**：内置了对常见网络攻击（如CSRF）的防护。

**项目结构与学习路径：**  
我们将分三章来逐步构建和完善这个Web应用：

+ **第18章：Django入门** - 搭建项目基础，包括设置开发环境、创建Django项目和应用、定义数据模型，并使用Django的管理后台添加初始数据。
+ **第19章：用户账户** - 实现用户功能，包括创建让用户添加和编辑数据的表单，以及构建完整的用户注册、登录和注销系统。
+ **第20章：设置应用程序的样式并部署** - 使用**Bootstrap**框架美化应用的外观，使其适应不同尺寸的设备（响应式设计），最后将整个项目**部署**到真实的Web服务器上，让任何人都可以通过互联网访问。

---

### **第18章：Django入门**
**本章核心目标：** 搭建Django项目的骨架，定义数据结构，并通过Django强大的管理后台初步与项目进行交互。

#### **18.1 建立项目**
1. **制定规范 (Spec)**：明确项目目标——一个让用户记录学习主题和笔记的在线日志系统。
2. **建立虚拟环境**：
    - **目的**：为项目创建一个隔离的Python环境，避免不同项目间的库版本冲突。这是专业开发的标准做法。
    - **命令**：

```bash
# 在项目主文件夹 learning_log 中
python -m venv ll_env       # 创建名为ll_env的虚拟环境
source ll_env/bin/activate  # 激活环境 (macOS/Linux)
# ll_env\Scripts\activate   # 激活环境 (Windows)
```

3. **安装Django**：

```bash
pip install django
```

4. **在Django中创建项目**：
    - **项目 (Project)**：是整个Web应用的配置和应用的集合。
    - **应用 (App)**：是项目中负责实现某个特定功能的模块，如博客、调查等。一个项目可以包含多个应用。
    - **命令**：

```bash
# 注意末尾的句点 .
django-admin startproject ll_project .
```

        * 这会创建一个名为 `ll_project` 的配置目录和一个 `manage.py` 工具文件。
5. **创建数据库**：
    - Django默认使用**SQLite**，这是一个轻量级的、基于单个文件的数据库。
    - **迁移 (Migrate)**：`migrate`命令会根据项目的模型设置来创建或更新数据库的结构。

```bash
python manage.py migrate
```

6. **查看项目**：
    - **开发服务器**：Django内置了一个用于开发的轻量级Web服务器。
    - **命令**：

```bash
python manage.py runserver
```

    - 在浏览器中访问 `http://localhost:8000/`，看到Django的欢迎页面，表示项目已成功创建。

#### **18.2 创建应用程序**
1. **创建**`learning_logs`**应用**：

```bash
python manage.py startapp learning_logs
```    *   这会创建一个名为 `learning_logs` 的新文件夹，里面包含了应用所需的基本文件结构。
```

2. **定义模型 (Models)**：

```python
# learning_logs/models.py
from django.db import models

class Topic(models.Model):
    """用户学习的主题"""
    text = models.CharField(max_length=200) # 1. 字符字段
    date_added = models.DateTimeField(auto_now_add=True) # 2. 日期时间字段

    def __str__(self):
        """返回模型的字符串表示"""
        return self.text

class Entry(models.Model):
    """学到的有关某个主题的具体知识"""
    topic = models.ForeignKey(Topic, on_delete=models.CASCADE) # 3. 外键
    text = models.TextField() # 4. 文本字段
    date_added = models.DateTimeField(auto_now_add=True)

    class Meta:
        verbose_name_plural = 'entries' # 5. 定义复数形式

    def __str__(self):
        return f"{self.text[:50]}..."
```

**细节解析：**

    - **文件**：`learning_logs/models.py`
    - **概念**：模型是数据的**唯一、确切的信息来源**。它是一个Python类，定义了数据的字段和行为。Django会根据模型自动创建数据库表。
    1. `CharField`：用于存储较短的文本，如标题。`max_length`是必需参数。
    2. `DateTimeField`：用于存储日期和时间。`auto_now_add=True`表示在创建对象时自动设置为当前时间。
    3. `ForeignKey`：**外键**，用于建立两个模型之间的**多对一**关系。这里表示一个`Entry`关联到一个`Topic`，而一个`Topic`可以有多个`Entry`。`on_delete=models.CASCADE`表示当关联的`Topic`被删除时，所有相关的`Entry`也会被级联删除。
    4. `TextField`：用于存储大量文本，没有长度限制。
    5. `Meta`类：用于存储模型的元数据。`verbose_name_plural`用于指定模型的复数形式，在管理后台中会用到。
3. **激活模型**：
    1. **在**`settings.py`**中注册应用**：将 `'learning_logs',` 添加到`INSTALLED_APPS`列表中。
    2. **为模型创建迁移**：`python manage.py makemigrations learning_logs`。Django会检测模型的改动，并生成一个迁移文件。
    3. **应用迁移**：`python manage.py migrate`。将迁移应用到数据库，创建新的数据表。
4. **Django管理网站**：
    - **创建超级用户**：`python manage.py createsuperuser`，然后按提示设置用户名和密码。
    - **注册模型**：在`learning_logs/admin.py`中注册你的模型，这样它们才会出现在管理后台中。

```python
# learning_logs/admin.py
from django.contrib import admin
from .models import Topic, Entry

admin.site.register(Topic)
admin.site.register(Entry)
```

    - **使用**：运行开发服务器，访问`http://localhost:8000/admin/`，用超级用户登录后，就可以通过图形界面来添加、修改和删除`Topic`和`Entry`了。

#### **18.3 创建网页**
创建网页通常分为三个步骤：定义URL、编写视图、创建模板。

1. **定义URL模式**：
    - **项目URL配置 (**`ll_project/urls.py`**)**：这是项目的总路由。我们需要在这里包含应用的URL配置。

```python
# ll_project/urls.py
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('learning_logs.urls')), # 包含应用的urls
]
```

    - **应用URL配置 (**`learning_logs/urls.py`**)**：为应用创建这个新文件，定义该应用内的具体URL。

```python
# learning_logs/urls.py
from django.urls import path
from . import views

app_name = 'learning_logs' # 1. 定义命名空间
urlpatterns = [
    # 主页
    path('', views.index, name='index'), # 2. URL模式
]
```

**细节解析：**

        1. `app_name`：定义了一个**命名空间**，可以防止不同应用之间URL名称的冲突。
        2. `path()`：第一个参数是URL模式（空字符串`''`匹配根URL），第二个参数是处理该URL的**视图函数**，第三个参数是该URL模式的**名称**。
2. **编写视图 (Views)**：
    - **文件**：`learning_logs/views.py`
    - **功能**：视图函数接收HTTP请求，处理业务逻辑，并返回一个HTTP响应（通常是渲染一个HTML页面）。

```python
# learning_logs/views.py
from django.shortcuts import render

def index(request):
    """学习笔记的主页"""
    return render(request, 'learning_logs/index.html')
```

        * `render()`函数会结合**数据（可选）****和****模板**来生成一个完整的HTML页面。
3. **创建模板 (Templates)**：
    - **文件结构**：在`learning_logs`应用目录下创建`templates/learning_logs/`文件夹，并将模板文件放在这里。
    - `base.html`** (父模板)**：创建一个基础模板，包含所有页面的公共部分（如页头、导航栏）。

```html
<!-- base.html -->
<p>
    <a href="{% url 'learning_logs:index' %}">Learning Log</a>
</p>
{% block content %}{% endblock content %}
```

    - `index.html`** (子模板)**：继承父模板，并填充具体内容。

```html
<!-- index.html -->
{% extends "learning_logs/base.html" %}

{% block content %}
    <p>Learning Log helps you keep track of your learning...</p>
{% endblock content %}
```

**细节解析：**

        * **模板标签 **`{% %}`：用于执行逻辑，如`extends`（继承）、`block`（定义块）。
        * `{% url 'namespace:name' %}`：这是Django模板中一个极其重要的标签，它会根据URL配置**动态地生成URL**。这比硬编码URL更可靠。

---

### **第19章：用户账户**
**本章核心目标：** 在项目上添加完整的用户系统，让用户可以注册、登录、注销，并确保用户只能看到和修改自己的数据。

#### **19.1 让用户能够输入数据**
1. **Django表单 (Forms)**
    - `ModelForm`：一种强大的工具，可以根据你定义的模型**自动生成表单**。
    - **创建**`forms.py`**文件**：

```python
# learning_logs/forms.py
from django import forms
from .models import Topic

class TopicForm(forms.ModelForm):
    class Meta:
        model = Topic  # 基于哪个模型
        fields = ['text'] # 表单中包含哪些字段
        labels = {'text': ''} # 为字段text生成一个空标签
```

2. **创建“添加新主题”的页面**：
    - **URL**：在`learning_logs/urls.py`中添加一个新路径，如`path('new_topic/', views.new_topic, name='new_topic')`。
    - **视图 **`new_topic()`：

```python
# learning_logs/views.py
from django.shortcuts import render, redirect
from .forms import TopicForm

def new_topic(request):
    if request.method != 'POST':
        form = TopicForm() # GET请求，创建空表单
    else:
        form = TopicForm(data=request.POST) # POST请求，用用户数据创建表单
        if form.is_valid(): # 验证数据
            form.save() # 保存到数据库
            return redirect('learning_logs:topics') # 重定向到主题列表页

    context = {'form': form}
    return render(request, 'learning_logs/new_topic.html', context)
```

        * 这个视图需要处理两种请求：**GET**（用户第一次访问页面，显示空表单）和**POST**（用户提交了填写好的表单）。
        * 通过`request.method`来区分。
    - **模板 **`new_topic.html`：

```html
<!-- new_topic.html -->
<form action="{% url 'learning_logs:new_topic' %}" method='post'>
    {% csrf_token %} <!-- 安全令牌，防止CSRF攻击 -->
    {{ form.as_p }} <!-- 将表单渲染为<p>标签 -->
    <button name="submit">Add topic</button>
</form>

```

#### **19.2 创建用户账户**
1. **创建**`accounts`**应用**：`python manage.py startapp accounts`。
2. **包含**`accounts`**的URL**：在项目主`urls.py`中添加`path('accounts/', include('accounts.urls'))`。
3. **使用Django内置的认证URL**：

```python
# accounts/urls.py
from django.urls import path, include

app_name = 'accounts'
urlpatterns = [
    # 包含默认的身份验证URL
    path('', include('django.contrib.auth.urls')),
]
```

    - 这会自动为我们提供登录 (`login`)、注销 (`logout`) 等页面的URL和视图。
4. **创建登录模板**：在`accounts/templates/registration/login.html`中创建模板。Django的认证系统会自动在这个路径下查找模板。
5. **创建注册页面**：
    - **URL**: 在`accounts/urls.py`中添加`path('register/', views.register, name='register')`。
    - **视图 **`register()`:

```python
# accounts/views.py
from django.contrib.auth import login
from django.contrib.auth.forms import UserCreationForm

def register(request):
    if request.method != 'POST':
        form = UserCreationForm()
    else:
        form = UserCreationForm(data=request.POST)
        if form.is_valid():
            new_user = form.save()
            login(request, new_user) # 注册后自动登录
            return redirect('learning_logs:index')
    # ...
```

    - **模板 **`register.html`: 结构与`login.html`类似。

#### **19.3 让用户拥有自己的数据**
1. **限制访问 (**`@login_required`**)**
    - **装饰器 (Decorator)**：是一种修改函数行为的简便方法。
    - 在`views.py`中，在需要保护的视图函数前加上`@login_required`，未登录的用户访问时会自动被重定向到登录页面。

```python
from django.contrib.auth.decorators import login_required

@login_required
def topics(request):
    # ...
```

2. **将数据关联到用户**：
    - **修改**`Topic`**模型**：添加一个外键，将其与`User`模型关联。

```python
# learning_logs/models.py
from django.contrib.auth.models import User
# ... in class Topic ...
owner = models.ForeignKey(User, on_delete=models.CASCADE)
```

    - **数据迁移**：修改模型后，必须再次运行`makemigrations`和`migrate`。
3. **只显示用户自己的数据**：
    - 在视图函数中，使用`.filter()`来查询只属于当前登录用户的数据。

```python
# learning_logs/views.py (in topics view)
topics = Topic.objects.filter(owner=request.user).order_by('date_added')
```

4. **确保新主题属于当前用户**：
    - 在`new_topic`视图中，保存表单前，将新主题的`owner`属性设置为当前用户。

```python
# ...
if form.is_valid():
    new_topic = form.save(commit=False) # 先不保存到数据库
    new_topic.owner = request.user # 设置所有者
    new_topic.save() # 再保存
    return redirect('learning_logs:topics')
```

5. **保护页面**：确保用户不能通过手动输入URL的方式访问或编辑**其他用户**的数据。在视图中进行检查：

```python
# ... in topic view ...
if topic.owner != request.user:
    raise Http404
```

---

### **第20章：设置应用程序的样式并部署**
**本章核心目标：** 美化我们的Web应用，并将其发布到互联网上，让全世界的人都能访问。

#### **20.1 设置样式 (Bootstrap)**
1. **Bootstrap简介**：一个流行的前端框架，提供了一套CSS样式和JavaScript组件，可以快速构建出美观且**响应式**（在手机、平板、电脑上都能良好显示）的网页。
2. **使用**`django-bootstrap5`：
    - 安装：`pip install django-bootstrap5`。
    - 在`settings.py`的`INSTALLED_APPS`中添加`'django_bootstrap5'`。
3. **修改模板**：
    - `base.html`：
        1. 在文件顶部加载`bootstrap5`标签：`{% load django_bootstrap5 %}`。
        2. 在`<head>`中加载CSS和JS文件：`{% bootstrap_css %}` 和 `{% bootstrap_javascript %}`。
        3. 使用Bootstrap提供的HTML结构和CSS类（如`navbar`, `container`, `btn`）来构建导航栏和页面布局。
    - **其他模板**：使用Bootstrap的CSS类（如`card`, `list-group`）来美化表单、列表等元素。
    - **表单样式**：使用`{% bootstrap_form form %}`标签可以自动为表单应用Bootstrap样式。

#### **20.2 部署 (Platform.sh)**
部署是将我们的本地项目上传到一台24小时运行的**Web服务器**上的过程。

1. **选择托管平台**：书中选择了Platform.sh，但原理适用于Heroku, AWS等其他平台。
2. **部署前的准备**：
    - `requirements.txt`：创建一个文件，列出项目的所有Python依赖库。

```bash
pip freeze > requirements.txt
```

    - **配置文件**：为部署平台创建特定的配置文件（如`.platform.app.yaml`），告诉平台如何构建和运行我们的Django项目。
    - **修改**`settings.py`：添加线上环境的特定配置，比如允许的主机名、数据库设置，并将`DEBUG`设为`False`（**极其重要，为了安全**）。
3. **使用Git进行版本控制**：
    - `git`：是一个版本控制系统，用于跟踪代码的修改历史。
    - **基本流程**：
        1. `git init`: 初始化一个本地仓库。
        2. 创建`.gitignore`文件，告诉Git忽略哪些文件（如虚拟环境、数据库文件）。
        3. `git add .`: 将所有文件添加到暂存区。
        4. `git commit -m "Initial commit"`: 创建一个提交（快照）。
4. **部署到Platform.sh**
    - **安装Platform.sh CLI**：安装平台的命令行工具。
    - **创建项目**：在Platform.sh上创建一个新项目。
    - **推送代码**：使用`platform push`（或`git push`）将你的本地Git仓库推送到Platform.sh的远程服务器上。
    - 平台会自动读取配置文件，安装依赖，运行迁移，并启动你的应用。
5. **创建定制错误页面**：创建`404.html`和`500.html`模板，当出现页面未找到或服务器内部错误时，向用户显示友好的错误页面，而不是调试信息。

---

#### **项目总结**
项目三是一个完整而深入的实践。通过它，我不仅学会了使用**Django**这个强大的框架来构建Web应用，还掌握了Web开发的整个生命周期：从**规划、建模、开发，到样式设计，最终部署上线**。我理解了**模型-视图-模板（MVT）这一核心设计模式，学会了如何处理用户输入和认证，并意识到了在生产环境中安全配置**（如`DEBUG=False`）的重要性。将项目部署到真实的服务器上，并获得一个公开的网址，这种成就感是前所未有的。这个项目为我进一步深入Web开发领域打下了坚实的基础。



