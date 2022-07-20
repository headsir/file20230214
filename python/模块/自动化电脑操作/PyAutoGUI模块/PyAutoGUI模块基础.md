# PyAutoGUI

# 一、模块安装

```python
pip install pyautogui
```

# 二、鼠标操作

整个桌面是以左上角为坐标轴的原点，所有的操作都以这个原点，来确定操作位置

## 2.1 鼠标移动

```python
# 当前屏幕分辨率
pyautogui.size()         # 输出的结果是：(1920,1080) （结果是当前屏幕分辨率）

# x,y是否在屏幕上
pyautogui.onScreen(x,y)      # 输出的结果是：True/False

# 移动鼠标,`duration`，这个参数表示移动时间，即在指定时间内完成移动操作，单位是秒
pyautogui.moveTo(200,400,duration=2)  # 将鼠标移动到指定的像素(200,400)位置
pyautogui.moveRel(200,500,duration=2)  # 将鼠标按照当前点向右移动200px，向下移动500px这个方向移动

# 获取鼠标位置
pyautogui.position()
```

## 2.2 鼠标点击

### 2.2.1 单击

```
# 鼠标点击，默认左键
pyautogui.click(100,100)   
# 单击左键
pyautogui.click(100,100,button='left')  
# 单击右键
pyautogui.click(100,300,button='right') 
# 单击中间 
pyautogui.click(100,300,button='middle')  
```

### 2.2.2 双击

```
# 双击左键
pyautogui.doubleClick(10,10)  
# 双击右键
pyautogui.rightClick(10,10)   
# 双击中键
pyautogui.middleClick(10,10) 
```

### 2.2.3 按下和释放

```
# 鼠标按下
pyautogui.mouseDown()   
# 鼠标释放
pyautogui.mouseUp()
```

### 2.2.4 鼠标拖动

我们可以控制鼠标拖动到指定坐标位置，并且设置操作时间：

```
pyautogui.dragTo(100,300,duration=1)   

pyautogui.dragRel(100,300,duration=4)  # 向右拖动
```

### 2.2.5 鼠标滚动

参数是整数，表示向上或向下滚动多少个单位，这个单位根据不同的操作系统可能不一样。如果向上滚动，传入正整数，向下滚动传入负整数。

```
pyautogui.scroll(30000) 
```

# 三、键盘操作

## 3.1 键盘输入

### 3.1.1 键盘函数

键盘输入有下面几个常用的函数：

- keyDown()：模拟按键按下
- keyUP()：模拟按键松开
- press()：模拟一次按键过程，即 keyDown 和 keyUP 的组合
- typewrite()：模拟键盘输出内容

```
# 输入感叹号（！）
pyautogui.keyDown('shift')    
pyautogui.press('1')    
pyautogui.keyUp('shift')   
```

直接输出内容：

```
pyautogui.typewrite('python', 1)  # 第一个参数是输出的内容，第二个参数是间隔时间，单位是秒。
```

### 3.1.2 特殊符号

有时我们需要输入键盘的一些特殊的符号按键，比如 换行、方向键等，这些有相对应的键盘字符串表示：

```
pyautogui.typewrite(['p','y','t','h','o','n','enter'])   
```

### 3.1.3 快捷键

如果我要复制一个内容，大部分情况下会使用，按照上面讲的，我们应该这么实现：

```
# 快键键 ctrl + c
pyautogui.keyDown('ctrl')
pyautogui.keyDown('c')
pyautogui.keyUp('c')
pyautogui.keyUp('ctrl')
# 推荐使用
pyautogui.hotkey('ctrl','c')  # 快捷函数
```

### 3.1.4 信息框

```
# 选择框
way = pyautogui.confirm('领导，该走哪条路？', buttons=['农村路', '水路', '陆路'])
print(way)
# 警告框
alert = pyautogui.alert(text='警告！敌军来袭！', title='警告框')
print(alert)
# 密码框
password = pyautogui.password('请输入密码')
print(password)
# 普通输入框
input = pyautogui.prompt('请输入指令：')
print(input)
```

# 四、屏幕处理

## 4.1 获取屏幕截图

```
# 获取屏幕截图函数，它可以返回一个 Pillow 的 image 对象

# region参数，截图区域，由左上角坐标、宽度、高度4个值确定，
#如果指定区域超出了屏幕范围，超出部分会被黑色填充，默认`None`，截全屏 
im = pyautogui.screenshot(region(0,0,300,400))
im.save('screenshot.png')

# 取屏幕截图中指定坐标点的颜色，返回 rgb 颜色值
rgb = im.getpixel((100, 500))
print(rgb)

# 将指定坐标点的颜色和目标的颜色进行比对，返回布尔值。
match = pyautogui.pixelMatchesColor(500,500,(12,120,400))
print(match)

 #返回图片中心的位置
x,y =pyautogui.locateCenterOnScreen('apple.png')     
```

## 4.2 图像识别

```
# 图像识别（一个）
oneLocation = pyautogui.locateOnScreen('1.png')
print(oneLocation)  

# 图像识别（多个）
allLocation = pyautogui.locateAllOnScreen('1.png')
print(list(allLocation))
```

图像识别成功率低解决方案：

        1.模糊定位：借助opencv的来提高识别率，在local函数中加入confidence参数，也就是识别准确度，当confidence越小时，定位的精度就会越低，从而实现模糊定位。【pip install opencv-python】
    
        2.灰度匹配：在local函数中加入grayscale参数，当grayscale=True时会使图像和屏幕截图中的颜色去饱和，可以解决由于显示器饱和度不同从而引起的颜色细微差异因而导致的图像定位失败问题。
    
        3.指定范围：在local函数中加入region参数，可以控制找图范围，从而提高找图效率。
    
        4.多图定位：icon在不同场景下可能有不同的显示效果，可以把不同显示效果的多张图片归为一个事件，对多张图进行循环查找，定位一张图就可以对整个事件进行定位。


