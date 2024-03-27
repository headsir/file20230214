# 1、项目使用的模块

```
requests>=2.31.0
openpyxl>=3.1.2
Flask>=3.0.0
qrcode>=7.4.2
emoji>=2.8.0
rich>=13.6.0
lxml>=4.9.3
```

## **requests**

​		Requests 库是一个简洁易用的 Python HTTP 库，它基于 `urllib3` 库，可以用来发送 HTTP  请求，并处理响应结果。Requests 库提供了一种更人性化的接口，让用户更容易地编写 HTTP 请求代码。相比于 Python 自带的  `urllib/urllib2` 库，Requests 功能更加完整且易用，而且支持多种协议和认证方式，具有更好的扩展性和可读性。

## **openpyxl**

​		` openpyxl`模块是一个读写Excel 文档的Python库，`openpyxl`是一个比较综合的工具，能够同时读取和修改Excel文档

## **Flask**

​		Flask是一个用Python编写的Web应用程序框架。 它由Armin Ronacher开发，他领导着一个名为Pocco的Python爱好者的国际组织。 Flask基于Werkzeug WSGI工具包和Jinja2模板引擎。

## **qrcode**

​		`qrcode` 是一个用于生成QR码（二维码）的Python库。

## **emoji**

​		`emoji` 模块是一个用于处理emoji字符的Python库。它可以帮助你将emoji编码为Unicode，反之亦然。

## **rich**

​		Rich 是一个 Python 库，可以为你在终端中提供富文本和漂亮、精美的格式。

## **lxml**

​		*lxml*是*python*的一个解析库，支持HTML和XML的解析

# 2、项目学习

## 2.1 公共变量

项目根目录

```python
from pathlib import Path
# resolve() 将路径设为绝对的，同时解析所有符号链接并将其规范化(例如在Windows下将斜杠转换为反斜杠)。
# .parent 父目录
PROJECT_ROOT = Path(__file__).resolve().parent
```

版本说明

```
VERSION_MAJOR = 5  # 主要版本
VERSION_MINOR = 3  # 小版本
VERSION_BETA = True  # 版本测试
```



```python
# 代码仓库
REPOSITORY = "https://github.com/JoeanAmier/TikTokDownloader"
# LICENCE 许可声明
LICENCE = "GNU General Public License v3.0"
# 文档的网址
DOCUMENTATION_URL = "https://github.com/JoeanAmier/TikTokDownloader/wiki/Documentation"
# 版本发布
RELEASES = "https://github.com/JoeanAmier/TikTokDownloader/releases/latest"
# 项目版本
NAME = f"TikTokDownloader v{VERSION_MAJOR}.{VERSION_MINOR}{' Beta' if VERSION_BETA else ''}"
# 宽度
WIDTH = 50
# 分割线
LINE = ">" * WIDTH
```



```python
# 更新
UPDATE = {"path": PROJECT_ROOT.joinpath("./src/config/Disable_Update")}
# 记录
RECORD = {"path": PROJECT_ROOT.joinpath("./src/config/Disable_Record")}
# 日志
LOGGING = {"path": PROJECT_ROOT.joinpath("./src/config/Enable_Logging")}
# 免责声明
DISCLAIMER = {"path": PROJECT_ROOT.joinpath("./src/config/Consent_Disclaimer")}
```



```python
# 免责声明内容
DISCLAIMER_TEXT = (
"关于 TikTokDownloader 的 免责声明：",
"",
"1. 使用者对本项目的使用由使用者自行决定，并自行承担风险。作者对使用者使用本项目所产生的任何损失、责任、或风险概不负责。",
"2. 本项目的作者提供的代码和功能是基于现有知识和技术的开发成果。作者尽力确保代码的正确性和安全性，但不保证代码完全没有错误或缺陷。",
"3. 使用者在使用本项目时必须严格遵守 GNU General Public License v3.0 的要求，并在适当的地方注明使用了 GNU General Public License v3.0 的代码。",
"4. 使用者在任何情况下均不得将本项目的作者、贡献者或其他相关方与使用者的使用行为联系起来，或要求其对使用者使用本项目所产生的任何损失或损害负责。",
"5. 使用者在使用本项目的代码和功能时，必须自行研究相关法律法规，并确保其使用行为合法合规。任何因违反法律法规而导致的法律责任和风险，均由使用者自行承担。",
"6. 本项目的作者不会提供 TikTokDownloader 项目的付费版本，也不会提供与 TikTokDownloader 项目相关的任何商业服务。",
"7. 基于本项目进行的任何二次开发、修改或编译的程序与原创作者无关，原创作者不承担与二次开发行为或其结果相关的任何责任，使用者应自行对因"
"二次开发可能带来的各种情况负全部责任。",
"",
"在使用本项目的代码和功能之前，请您认真考虑并接受以上免责声明。如果您对上述声明有任何疑问或不同意，请不要使用本项目的代码和功能。如果"
"您使用了本项目的代码和功能，则视为您已完全理解并接受上述免责声明，并自愿承担使用本项目的一切风险和后果。",
"",

)
```

## 2.2 项目 `__init__()`方法

```
def __init__(self):
	# 实例化控制台样式对象
    self.console = ColorfulConsole()
    # 实例化日志对象
    self.logger = None
    self.blacklist = None
    self.user_agent, self.ua_code = Headers.generate_user_agent()
    self.x_bogus = XBogus()
    self.settings = Settings(self.PROJECT_ROOT, self.console)
    self.cookie = Cookie(self.settings, self.console)
    self.register = Register(
    self.settings,
    self.console,
    self.x_bogus,
    self.user_agent,
    self.ua_code)
    self.parameter = None
    self.running = True
    self.event = Event()
    self.cookie_task = Thread(target=self.periodic_update_cookie)
    self.backup_task = None
    self._abnormal = None
```

控制台的样式

```python
class ColorfulConsole(Console):
    def print(self, *args, style=GENERAL, highlight=False, **kwargs):
        super().print(*args, style=style, highlight=highlight, **kwargs)

    def input(self, prompt_="", *args, **kwargs):
        return super().input(
            f"[{PROMPT}]{prompt_}[/{PROMPT}]", *args, **kwargs)
```

