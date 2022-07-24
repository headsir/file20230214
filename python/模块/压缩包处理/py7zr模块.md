py7zr模块

# 模块安装

pip install py7zr

# 模块使用

```
# 解压缩
import py7zr
a = py7zr.SevenZipFile(r'd:/桌面/新建文件夹.7z','r')
a.extractall(path=r'd:/桌面/新建文件夹')
a.close()
```