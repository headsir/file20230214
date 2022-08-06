# python-pptx操作PPT

安装模块

```
pip install python-pptx
```

# 基础知识

## 获取PPT对象

```
from pptx import Presentation
# 打开PPT
ppt = Presentation('python-pptx 多页待删除模板.pptx')
# 新建PPT
ppt = Presentation()
```

## 获取PPT页

```
# PPT页数量
slides = ppt.slides  # PPT幻灯片集合
number_pages = len(slides)

# 第n_page+1张PPT页
slide = ppt.slides[n_page]
```

## 保存PPT

```
ppt.save('python-pptx 多页已删除模板.pptx')
```

## 删除PPT页

```
def del_slide(prs,index):
	# PPT页集合
    slides = list(prs.slides._sldIdLst)
    # 移除PPT页
    ppt.slides._sldIdLst.remove(slides[index])  # index从0开始
```

## 添加ppt页

```
slide = ppt.slides.add_slide(ppt.slide_layouts[0])  # 0代表幻灯片模板
```

## 添加文本框

```
from pptx.util import Cm
# 设置添加文字框的位置以及大小
# left:float,水平位置
# top:float,垂直位置
# width:float,宽度
# height:float,高度
left, top, width, height = Cm(16.9), Cm(3), Cm(12), Cm(3.6)
slide.shapes.add_textbox(left=left, top=top, width=width, height=height)
```

### 调整文本框背景颜色

```
# 调整文本框背景颜色
textBoxFill = textBox.fill
textBoxFill.solid()  # 纯色填充
textBoxFill.fore_color.rgb = RGBColor(187, 255, 255)  # 背景颜色
```

### 文本框边框样式调整

```
from pptx.dml.color import RGBColor
# 文本框边框样式调整
line = textBox.line
line.color.rgb = RGBColor(0, 255, 0)  # 颜色
line.width = Cm(0.1)  # 线宽
# RGB颜色参考：http://www.wahart.com.hk/rgb.htm
```

### 获取文本框对象

```
# 获取文本框对象
tf = textBox.text_frame
```

### 文本框文字样式调整

```
from pptx.enum.text import MSO_VERTICAL_ANCHOR
# 文本框样式调整
tf.margin_bottom = Cm(0.1)  # 下边距
tf.margin_left = 0  # 左边距
tf.vertical_anchor = MSO_VERTICAL_ANCHOR.BOTTOM  # 垂直方式：底端对齐
tf.word_wrap = True  # 文本框的文字自动对齐

"""
垂直方式
TOP
	Aligns text to top of text frame and inherits its value from its layout placeholder or theme.
MIDDLE
	Centers text vertically
BOTTOM
	Aligns text to bottom of text frame
MIXED
	Return value only; indicates a combination of the other states.
"""
```

### 设置文本框内容

```
# 设置内容
tf.paragraphs[0].text = '这是一段文本框里的文字'
```

### 字体样式调整

```
from pptx.enum.text import PP_ALIGN
# 字体样式调整
tf.paragraphs[0].alignment = PP_ALIGN.CENTER  # 对齐方式
tf.paragraphs[0].font.name = '微软雅黑'  # 字体名称
tf.paragraphs[0].font.bold = True  # 是否加粗
tf.paragraphs[0].font.italic = True  # 是否斜体
tf.paragraphs[0].font.color.rgb = RGBColor(255, 0, 0)  # 字体颜色
tf.paragraphs[0].font.size = Pt(20)  # 字体大小

"""
文字对齐方式
 CENTER
  	Center align
  DISTRIBUTE
  	Evenly distributes e.g. Japanese characters from left to right within a line
  JUSTIFY
  	Justified, i.e. each line both begins and ends at the margin with spacing between words adjusted such that the line exactly fills the width of the paragraph.
  JUSTIFY_LOW
  	Justify using a small amount of space between words.
  LEFT
  	Left aligned
  RIGHT
  	Right aligned
  THAI_DISTRIBUTE
  	Thai distributed
  MIXED
  	Return value only; indicates multiple paragraph alignments are present in a set of paragraphs.
 """
```





```
4.1 python-pptx 添加文字并设置样式
4.1.1 添加单行文字与多行文字

示例代码：

from pptx import Presentation
from pptx.util import Pt,Cm

# 打开已存在ppt
ppt = Presentation('4. python-pptx操作模板.pptx')

# 设置添加到当前ppt哪一页
n_page = 0
singleLineContent = "我是单行内容"
multiLineContent = \
"""我是多行内容1
我是多行内容2
我是多行内容3
"""

# 获取需要添加文字的页面对象
slide = ppt.slides[n_page]

# 添加单行内容

# 设置添加文字框的位置以及大小
left, top, width, height = Cm(16.9), Cm(1), Cm(12), Cm(1.2)
# 添加文字段落
new_paragraph1 = slide.shapes.add_textbox(left=left, top=top, width=width, height=height).text_frame
# 设置段落内容
new_paragraph1.paragraphs[0].text = singleLineContent
# 设置文字大小
new_paragraph1.paragraphs[0].font.size = Pt(15)

# 添加多行

# 设置添加文字框的位置以及大小
left, top, width, height = Cm(16.9), Cm(3), Cm(12), Cm(3.6)
# 添加文字段落
new_paragraph2 = .text_frame
# 设置段落内容
new_paragraph2.paragraphs[0].text = multiLineContent
# 设置文字大小
new_paragraph2.paragraphs[0].font.size = Pt(15)

# 保存ppt
ppt.save('4.1 添加文字.pptx')
```

