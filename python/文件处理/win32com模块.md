# win32com模块

模块及版本要求：pywin32==227

```
# coding=utf-8

import win32com.client

class ExcelWin(object):
    def __init__(self, file_path):
        """
        定义操作文件路径
        :param file_path: 操作文件路径
        """
        self.file_path = file_path

    def excel_start(self):
        """
        启动Excel程序，并打开需要操作得文件
        """
        try:
            self.xlApp = win32com.client.DispatchEx("Excel.Application")
            self.xlApp.Visible = False  # 显示EXCEL界面
            self.xlApp.DisplayAlerts = False
            self.xlBook = self.xlApp.Workbooks.Open(self.file_path, False)  # 打开EXCEL文件
            run_result = "Excel表文件打开成功"
        except:
            run_result = "Excel表文件打开失败"
        return run_result

    def save_quit(self):
        """
        保存文件，关闭文件，退出程序
        """
        try:
            self.xlBook.Save()
            self.xlBook.Close()
            self.xlApp.Quit()

            run_result = "{}保存成功".format(self.file_path)
        except:
            run_result = "{}保存失败".format(self.file_path)
        return run_result

    def vba(self, vba):
        """
        运行文件中VBA程序
        :param vba:VBA程序名称
        :return:运行结果
        """
        try:
            self.excel_start()
            self.xlBook.Application.Run(vba)
            run_result = "{}运行成功".format(vba)
        except:
            run_result = "{}运行失败".format(vba)
        finally:
            self.save_quit()
        return run_result
        
	def excel_look_cell(self, sheet, rang):
        """
        更新excel表单元格内容
        :param sheet:sheet名字
        :param rang:单元格
        :return:run_result 运行结果 ，value 查询结果
        """
        value = None
        try:
        self.excel_start()
        sht = self.xlBook.Worksheets(sheet)  # 打开对应名称的sheet
        value = sht.Range(rang).Value
        run_result = "{0}中{1}表格【{2}】读取成功".format(self.file_path, sheet, rang)
        except:
        run_result = "{0}中{1}表格【{2}】读取失败".format(self.file_path, sheet, rang)
        finally:
        self.save_quit()
        return run_result, value

    def excel_update_cell(self, sheet, rang, value):
        """
        更新excel表单元格内容
        :param sheet:sheet名字
        :param rang:单元格
        :param value:更新值
        :return:
        """
        try:
            self.excel_start()
            sht = self.xlBook.Worksheets(sheet)  # 打开对应名称的sheet
            sht.Range(rang).Value = value
            run_result = "{0}中{1}表格{2}更新成功".format(self.file_path, sheet, rang)
        except:
            run_result = "{0}中{1}表格{2}更新失败".format(self.file_path, sheet, rang)
        finally:
            self.save_quit()
        return run_result

    def excel_delete_cell(self, sheet, start_row):
        """
        按照表格行删除最后几行数据
        :param sheet: sheet表名字
        :param start_row: 起始行
        :return:
        """
        nrows = None
        try:
            self.excel_start()
            sht = self.xlBook.Worksheets(sheet)  # 打开对应名称的sheet
            nrows = sht.UsedRange.Rows.Count  # 获取使用区域的行数
            if nrows >= start_row:
                sht.Rows("{}:{}".format(start_row, nrows)).ClearContents()  # 对当前使用区域清除内容
                run_result = "{0}中{1}行{2}到{3}删除成功".format(self.file_path, sheet, start_row, nrows)
            else:
                run_result = "最大行{1}小于起始行{0}，删除失败".format(start_row, nrows)
        except:
            run_result = "{0}中{1}行{2}到{3}，程序异常，删除失败".format(self.file_path, sheet, start_row, nrows)
        finally:
            self.save_quit()
        return run_result

    # 其它
    """
     sht.UsedRange.ClearContents()  # 对当前使用区域清除内容
     nrows = sht.UsedRange.Rows.Count  # 获取使用区域的行数
     sht.UsedRange.Copy()  # 复制
     sht.Activate()  # 激活当前工作表
    """


if __name__ == '__main__':
    path = r"D:\桌面\结算问题.xlsx"
    excel = ExcelWin(path)
    excel.excel_update_cell(sheet="sheet2", rang="A2:D10", value="10")
```