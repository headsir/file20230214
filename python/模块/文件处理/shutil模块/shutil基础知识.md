

# 复制文件到指定目录

```python
import shutil
import pandas as pd

file_path = r"D:\桌面\七期室分96011"  # 指定目录
# 存放有源文件文件名+文件路径:E:\1-工程资料-勿修改\工程单验报告\4G\室分\郑州电信地铁3号线兴隆铺站站厅台测试报告.docx
data = pd.read_excel(r"D:\桌面\file.xlsx")
for i in range(data.shape[0]):  # data.shape[0],data行数
    file_y = data.iloc[i, 0]  # 每行第一列内容，iloc行号，loc行标签
    shutil.copy2(file_y, file_path)  # shutil.copy2(源文件,指定目录)，复制到指定目录
```

