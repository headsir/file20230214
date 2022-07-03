# win32com操作excel

```
import win32com.client  # 模块pywin32==227

def useVBA(VBA):
    xlApp = win32com.client.DispatchEx("Excel.Application")
    xlApp.Visible = True
    xlApp.DisplayAlerts = 0
    xlBook = xlApp.Workbooks.Open(file_path, False)
    xlBook.Application.Run(VBA)  # 宏,VBA宏名字
    xlBook.Save()
    xlBook.Close()
    xlApp.Quit()
file_path = r"D:\用户\文档\EXCEL宏\工参一键生成工具V.1.1\5G工参一键生成v7.xlsm"
useVBA('工参更新')

def win_32_add(self, file_path, sheet, rang, value):
	xlApp = win32com.client.DispatchEx("Excel.Application")
    xlApp.Visible = False  # 显示EXCEL界面
    xlApp.DisplayAlerts = False
    xlBook = xlApp.Workbooks.Open(file_path, False)  # 打开EXCEL文件
    sht = xlBook.Worksheets(sheet)  # 打开对应名称的sheet
    sht.Range(rang).Value = value #单元格赋值
   	# sht.UsedRange.ClearContents()  # 对当前使用区域清除内容
    # nrows = sht.UsedRange.Rows.Count  # 获取使用区域的行数
    # sht.UsedRange.Copy()  # 复制
    # sht.Activate()  # 激活当前工作表
    # xlBook.Application.Run(VBA)  # 宏
    xlBook.Save()
    xlBook.Close()
    xlApp.Quit()
```

