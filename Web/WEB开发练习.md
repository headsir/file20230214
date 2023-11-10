# 案例：题库管理系统(QBMSys)

英文名：question bank management syste

简称：QBMSys

---

## 开发环境：

- python版本：3.9.13

- django版本：4.2.7

## 创建项目(QBMSys)

必须在CDM窗口运行。

```
"D:\ProgramData\virtual directory\python_learing-uJLLoHwc\Scripts\django-admin.exe" startproject QBMSys
```

## 本地配置

- 创建自己本地 local_settings.py

  在settings.py文件最后导入

  ```python
  try:
      from .local_settings import *
  except ImportError:
      pass
  ```

  local_settings.py中设置

  ```python
  # 语言配置
  LANGUAGE_CODE = 'zh-hans'
  # 时区配置
  TIME_ZONE = 'Asia/Shanghai'  # 亚洲/上海
  ```

  备注：时区问题 可参考：https://blog.csdn.net/qq_41341757/article/details/109319850

  

- git忽略文件.gitignore配置

  ```
  # pycharm 自动生成的目录
  .idea/
  
  # python缓存文件
  __pycache__/
  *.py[cod]
  *.$py.calss
  
  
  # Django stuff:
  local_settings.py
  *.sqlite3
  
  # database migrations
  **/migrations/*.py
  !**/migrations/__init__.py
  ```

## 创建app

采用多app模式。





## 系统构思

注册功能

登录功能

题库展示

题库导入

试题学习

试题练习

错题收集

错题练习

## 注册功能

## 登录功能