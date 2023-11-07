

# 一、加载课件

![image-20220628231233435](imge/案例一：中兴E学院课件下载.assets/image-20220628231233435.png)

## 二、复制网页源码

![image-20220628231353527](imge/案例一：中兴E学院课件下载.assets/image-20220628231353527.png)

复制内容保存为html文件。

### 三、爬取课件并保存

爬虫代码：适用于非视频课件

```

import random
import time,os
import re
import requests

headers = {
    "User-Agent": "Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Mobile Safari/537.36 Edg/91.0.864.37"
    # "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36"
}

"""
# has_special_char('Hello World!') # 此字符串中包含一个特殊字符“！”
# has_special_char('Hello123') # 此字符串中不包含任何特殊字符。
# https://deepinout.com/python/python-programs/t_program-to-check-if-a-string-contains-any-special-character-in-python.html?action=all
"""
pattern = re.compile(r'id="(.*?)"><img.*?src="(.*?) "', re.I | re.M | re.DOTALL)  # 课件图片网址
t = open("1.html", "r", encoding='utf-8')
html = t.read()
result = pattern.findall(html)
print(result)

# 创建文件目录
Path = "D:/桌面/VoNR综合优化方案PPT20231107"
if os.path.exists(path=Path) == False:
    os.mkdir(path=Path)  # 创建路径中的最后一级目录
else:
    print('directory exist')  # 目录存在
os.chdir(path=Path)  # os.chdir() 方法用于改变当前工作目录到指定的路径

for png_name, png1 in result:
    png = png1.replace("amp;", "")
    print(png_name, png)
    with open(f'{png_name}.png', 'wb') as v:
        try:
            # ===随机生产爬虫时间间隔==
            a = random.uniform(0.01, 0.1)
            print(a)
            time.sleep(a)
            # =====================
            v.write(requests.get(url=png, headers=headers).content)
        except Exception as e:
            print('download error')
```

