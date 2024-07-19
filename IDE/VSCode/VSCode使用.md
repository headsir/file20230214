# 插件

## Error Lens插件

是一款把代码检查（错误、警告、语法问题）进行突出显示的一款插件

![image-20240621221435408](imge/VSCode使用.assets/image-20240621221435408.png)

## One Dark Pro

颜色主题插件

![image-20240621220357631](imge/VSCode使用.assets/image-20240621220357631.png)

## Live Server

实时加载功能的小型服务器插件

![image-20240621221417799](imge/VSCode使用.assets/image-20240621221417799.png)

## Chinese (Simplified) (简体中文) 

界面汉化插件

![image-20240626090538371](imge/VSCode使用.assets/image-20240626090538371.png)

## vscode-icons

 设置文件图标主题

![image-20240626090846290](imge/VSCode使用.assets/image-20240626090846290.png)

## autoDocstring

实现函数注释

![image-20240626090933636](imge/VSCode使用.assets/image-20240626090933636.png)

## Todo Tree

 实现TODO 标签功能

![image-20240626091001811](imge/VSCode使用.assets/image-20240626091001811.png)

## Markdown Preview Enhanced 

实现Markdown的一些功能

![image-20240626091023434](imge/VSCode使用.assets/image-20240626091023434.png)



## indent-rainbow 

缩进渲染

![image-20240626091042550](imge/VSCode使用.assets/image-20240626091042550.png)

```
// 取消插件indent-Rainbow缩进渲染报错
"indentRainbow.ignoreErrorLanguages": [
"python"
]
```

## Black Formatter

自动格式化 Python 代码，不好用

![image-20240626141326533](imge/VSCode使用.assets/image-20240626141326533.png)

```
// 启用black格式化插件
"editor.defaultFormatter": "ms-python.black-formatter",
```









# 配置

2种方式：本项目设置、全局设置

## 保存自动格式化文件

![image-20240621220918038](imge/VSCode使用.assets/image-20240621220918038.png)

## 代码缩进字符修改

![image-20240621221154296](imge/VSCode使用.assets/image-20240621221154296.png)

## python 文件头模板

	// python 文件头模板
	"HEADER": {
		"prefix": "header",
		"body": [
			"#!/usr/bin/env python",
			"# -*- encoding: utf-8 -*-",
			"# @File    :   $TM_FILENAME",
			"# @Time    :   $CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND",
			"# @Author  :   978345836@qq.com",
			"# @Version :   1.0",
			"# @Describe:   None",
			"",
			"# here put the import lib",
			"$0"



# 快捷操作：

```
Alt + ↑/↓  移动行
Ctrl + C 复制行
Ctrl + V 粘贴行
Ctrl + 单击 多光标选择
Ctrl + D 添加下一个匹配项
```



# 创建虚拟环境

## venv方式

![image-20240626091258848](imge/VSCode使用.assets/image-20240626091258848-17193643807471.png)



# git 使用

### 配置git.exe

注：不能配置到项目目录下，配置在用户配置下

![image-20240719102446177](imge/VSCode使用.assets/image-20240719102446177.png)

![image-20240719102518278](imge/VSCode使用.assets/image-20240719102518278.png)

### 配置本地仓库

![image-20240719102913129](imge/VSCode使用.assets/image-20240719102913129.png)

![image-20240719102938241](imge/VSCode使用.assets/image-20240719102938241.png)

### 配置远程仓库

![image-20240719103325250](imge/VSCode使用.assets/image-20240719103325250.png)

![image-20240719103412640](imge/VSCode使用.assets/image-20240719103412640.png)

### 项目配置文件

`.gitignore`

忽略提交

```
# pycharm 自动生成的目录
.idea/

# python缓存文件
__pycache__/
*.py[cod]
*.$py.calss

# 配置文件
.vscode
.gitignore
# 数据文件
*.db

# Django stuff:
local_settings.py
*.sqlite3

# database migrations
**/migrations/*.py
!**/migrations/__init__.py

# 临时文件
1.*
*.xlsx
项目不相关文件，临时
```

