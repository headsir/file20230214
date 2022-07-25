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

推荐：CDM 调用7Z程序使用<D:\用户\文档\GitHub\study-notes\python\文件处理\案例\案例四：调用7zip命令解压缩.md>