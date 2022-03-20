# PyQt5使用

<font color = red size =5>注意事项：PyQt5安装路径不能包含中文字符</font>

## 一、模块安装

`pip PyQt5`

## 二、PyQT文件对话框使用

QFileDialog.getOpenFileName()：获取单个文件路径 

QFileDialog.getOpenFileNames()：获取多个文件路径

QFileDialog.getExistingDirectory()：获取文件夹路径

QFileDialog.getSaveFileName()  :选择保存文件

```python
 def slot_btn_chooseDir(self):
        dir_choose = QFileDialog.getExistingDirectory(self,
                                    "选取文件夹",
                                    self.cwd) # 起始路径

        if dir_choose == "":
            print("\n取消选择")
            return

        print("\n你选择的文件夹为:")
        print(dir_choose)


    def slot_btn_chooseFile(self):
        fileName_choose, filetype = QFileDialog.getOpenFileName(self,
                                    "选取文件",
                                    self.cwd, # 起始路径
                                    "All Files (*);;Text Files (*.txt)")   # 设置文件扩展名过滤,用双分号间隔

        if fileName_choose == "":
            print("\n取消选择")
            return

        print("\n你选择的文件为:")
        print(fileName_choose)
        print("文件筛选器类型: ",filetype)


    def slot_btn_chooseMutiFile(self):
        files, filetype = QFileDialog.getOpenFileNames(self,
                                    "多文件选择",
                                    self.cwd, # 起始路径
                                    "All Files (*);;PDF Files (*.pdf);;Text Files (*.txt)")

        if len(files) == 0:
            print("\n取消选择")
            return

        print("\n你选择的文件为:")
        for file in files:
            print(file)
        print("文件筛选器类型: ",filetype)


    def slot_btn_saveFile(self):
        fileName_choose, filetype = QFileDialog.getSaveFileName(self,
                                    "文件保存",
                                    self.cwd, # 起始路径
                                    "All Files (*);;Excel Files (*.xlsx);;Text Files (*.txt)")

        if fileName_choose == "":
            print("\n取消选择")
            return

        print("\n你选择要保存的文件为:")
        print(fileName_choose)
        print("文件筛选器类型: ",filetype)

```

