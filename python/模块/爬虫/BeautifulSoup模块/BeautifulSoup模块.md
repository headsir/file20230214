# BeautifulSoup模块

作用：解析网页

# 一、模块安装

```
pip install beautifulsoup4
```

## 二、BeautifulSoup 常用的API

在 BeautifulSoup 中主要使用的对象是 BeautifulSoup 实例，BeautifulSoup 中常用函数如下：

> - find_all(tagname)：根据HTML标签名返回所有符合条件的元素列表。
> - find(tagname)：根据HTML标签名返回符合条件的第一个元素。
> - select(selector)：通过 CSS 中的选择器查找符合条件的所有元素。
> - get(key, default = None)：获取标签的属性值，key 是标签的属性名。

BeautifulSoup 常用的属性如下：

> - title：获取当前 HTML 页面的 title 属性值。
> - text：返回标签中的文本内容。

```
import urllib.request
from bs4 import BeautifulSoup

# 请求数据的URL网址

url = "http://quotes.money.163.com/trade/lsjysj_{0}.html?year={1}&season={2}"

# 传递参数获得最终URL网址
strURL = url.format('600519', '2021', '1')  # 股票代码、年、季度
print("请求的URL：", strURL)

req = urllib.request.Request(strURL)

with urllib.request.urlopen(req) as response:
    dataStr = response.read().decode(encoding='utf-8', errors='ignore')
    # 创建BeautifulSoup对象
    sp = BeautifulSoup(dataStr,'html.parser')
    # 返回包含数据的表格table中所有行，即所有tr标签对象
    # trList = sp.select('.table_bg001 > thead')
    trList = sp.select('.table_bg001 > tr')
    print(trList)

    # 保存数据列表
    datas = []
    for tr in trList:
        # 找tr标签下面所有的td标签
        tds = tr.findAll('td')

        # 保存一行数据的字典对象
        row = dict()

        # 日期
        row['Date'] = tds[0].text
        # 开盘价
        row['Open'] = float(tds[1].text.replace(',',''))
        # 最高价
        row['High'] = float(tds[2].text.replace(',',''))
        # 最低价
        row['Low'] = float(tds[3].text.replace(',',''))
        # 收盘价
        row['Close'] = float(tds[4].text.replace(',',''))
        # 成交量
        row['Volume'] = float(tds[7].text.replace(',',''))
        datas.append(row)

    # 测试一下爬虫回来的数据
    print(datas)
```

