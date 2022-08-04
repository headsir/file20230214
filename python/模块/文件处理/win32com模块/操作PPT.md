# win32com操作ppt

官方文档：https://docs.microsoft.com/zh-cn/office/vba/api/powerpoint.shape.copy

# 案例学习

## 一、win32com复制ppt模板

```
import win32com
from win32com.client import Dispatch
import os

ppt = Dispatch('PowerPoint.Application')
# 或者使用下面的方法，使用启动独立的进程：
# ppt = DispatchEx('PowerPoint.Application')

# 如果不声明以下属性，运行的时候会显示的打开word
ppt.Visible = 1  # 后台运行
ppt.DisplayAlerts = 0  # 不显示，不警告

# 创建新的PowerPoint文档
# pptSel = ppt.Presentations.Add() 
# 打开一个已有的PowerPoint文档
pptSel = ppt.Presentations.Open(os.getcwd() + "\\" + "2.2 win32 ppt测试.pptx")

# 复制模板页
pptSel.Slides(1).Copy()
#设置需要复制的模板页数
pageNums = 10
# 粘贴模板页
for i in range(pageNums):
    pptSel.Slides.Paste()

# pptSel.Save()  # 保存
pptSel.SaveAs(os.getcwd() + "\\" + "win32_copy模板.pptx")  # 另存为
pptSel.Close()  # 关闭 PowerPoint 文档
ppt.Quit()  # 关闭 office
```

# 封装模块
