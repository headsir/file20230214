[来源]https://cuiqingcai.com/1319.html

# BeautifulSoup模块

作用：解析网页

# 一、模块安装

```
pip install beautifulsoup4
```

# 二、模块导入

```python
导入 bs4 库
from bs4 import BeautifulSoup
```

# 三、创建 beautifulsoup 对象

```python
soup = BeautifulSoup(html)
# 本地HTML文件
soup = BeautifulSoup(open("html/百度新闻——海量中文资讯平台.html",encoding="utf8"))

# 打印页面
print (soup.prettify())
```

#  四、四大对象种类

​        Beautiful Soup 将复杂 HTML 文档转换成一个复杂的树形结构，每个节点都是 Python 对象，所有对象可以归纳为 4 种:      

- Tag        
-  NavigableString        
- BeautifulSoup        
- Comment        

## 4.1 Tag

注意：查找的是在所有内容中的第一个符合要求的标签

```python
# 打印label标签
soup.label  # <label class="checked" for="news">新闻全文</label>
```

### name属性

soup 对象本身比较特殊，它的 name 即为 [document]，对于其他内部标签，输出的值便为标签本身的名称

```python
soup.name  # '[document]'
soup.label.name  # 'label'
```

### attrs属性

输出值为标签属性，可以对属性值就行修改删除。

```python
soup.label.attrs
```

```python
{'for': 'news', 'class': ['checked']}
```

## 4.2 NavigableString

获取标签内部的文字

```python
soup.label.string  # 新闻全文
type(soup.label.string)  # bs4.element.NavigableString
```

## 4.3 BeautifulSoup

BeautifulSoup 对象表示的是一个文档的全部内容。大部分时候，可以把它当作 Tag 对象，是一个特殊的 Tag，我们可以分别获取它的类型，名称

```python
soup.name  # '[document]'
soup.attrs # {}
```

## 4.4 Comment

Comment 对象是一个特殊类型的 NavigableString 对象，其实输出的内容仍然不包括注释符号，但是如果不好好处理它，可能会对我们的文本处理造成意想不到的麻烦。 我们找一个带注释的标签

```python
soup.a
<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>

soup.a.string  # ' Elsie '
type(soup.a.string)  # bs4.element.Comment
```

使用前最好做一下判断

```python
if type(soup.a.string)==bs4.element.Comment:
    print soup.a.string
```

# 五、遍历文档树

## 5.1 直接子节点

### contents属性

将 tag 的子节点以列表的方式输出

```
soup.body.li.a.contents
```

```
['百度新闻客户端\n                ',
 <div id="app_tooltip_qrcode">
 <img src="%E7%99%BE%E5%BA%A6%E6%96%B0%E9%97%BB%E2%80%94%E2%80%94%E6%B5%B7%E9%87%8F%E4%B8%AD%E6%96%87%E8%B5%84%E8%AE%AF%E5%B9%B3%E5%8F%B0_files/newErweima_9fa03e0.png"/>
 </div>,
 '\n']
```

### **children属性**

将 tag 的子节点以 list 生成器对象的方式输出

```
soup.body.li.a.children  # <list_iterator at 0x1aac32e6d30>
```

```
for child in  soup.body.li.a.children:
    print (child)
    
# =========================================================    
百度新闻客户端
                
<div id="app_tooltip_qrcode">
<img src="%E7%99%BE%E5%BA%A6%E6%96%B0%E9%97%BB%E2%80%94%E2%80%94%E6%B5%B7%E9%87%8F%E4%B8%AD%E6%96%87%E8%B5%84%E8%AE%AF%E5%B9%B3%E5%8F%B0_files/newErweima_9fa03e0.png"/>
</div>
```

## 5.2 **所有子孙节点**

对所有 tag 的子孙节点进行==递归循环==

```
soup.body.li.a.descendants  # <generator object Tag.descendants at 0x000001AAC333E430>
```

```
for child in soup.body.li.a.descendants:
    print (child)
```

```html
百度新闻客户端
                
<div id="app_tooltip_qrcode">
<img src="%E7%99%BE%E5%BA%A6%E6%96%B0%E9%97%BB%E2%80%94%E2%80%94%E6%B5%B7%E9%87%8F%E4%B8%AD%E6%96%87%E8%B5%84%E8%AE%AF%E5%B9%B3%E5%8F%B0_files/newErweima_9fa03e0.png"/>
</div>

<img src="%E7%99%BE%E5%BA%A6%E6%96%B0%E9%97%BB%E2%80%94%E2%80%94%E6%B5%B7%E9%87%8F%E4%B8%AD%E6%96%87%E8%B5%84%E8%AE%AF%E5%B9%B3%E5%8F%B0_files/newErweima_9fa03e0.png"/>
```

## 5.3 **节点内容**

​		如果一个标签里面没有标签了，那么 .string 就会返回标签里面的内容。如果标签里面只有唯一的一个标签了，那么 .string 也会返回最里面的内容

​		如果 tag 包含了多个子节点，tag 就无法确定，string 方法应该调用哪个子节点的内容，.string 的输出结果是 None

```
print(soup.body.string)  # None
```

## 5.4 **多个内容**

### strings属性

```
for string in soup.body.strings:
    print(string)
```

### stripped_strings属性

可以去除多余空白内容

```
for string in soup.body.stripped_strings:
    print(string)
```

## 5.5 **父节点**

### parent 属性

```
soup.div.parent.name  # 'body'
```

## 5.6 全部父节点

### parents属性

通过元素的 .parents 属性可以递归得到元素的所有父辈节点

```
for parent in soup.li.parents:
    print(parent.name)
    
# ================================
ul
div
div
body
html
[document]
```

## 5.7 兄弟节点

和本节点处在同一级的节点，空白或者换行也可以被视作一个节点，所以得到的结果可能是空白或者换行，如果节点不存在，则返回 None

### next_sibling属性

获取该节点的下一个兄弟节点

```
print(soup.a.next_sibling.next_sibling)

<meta content="text/html; charset=utf-8" http-equiv="Content-Type"/>
```

### previous_sibling属性

获取该节点的上一个兄弟节点

```
print(soup.body.div.previous_sibling.previous_sibling)

<a aria-label="欢迎进入 百度新闻——海量中文资讯平台,盲人用户进入智能盲道请按快捷键Ctrl+Alt+R；阅读详细操作说明请按快捷键Ctrl+Alt+问号键。" href="javascript:void(0)" id="ariaTipText" role="pagedescription"></a>
```

## 5.8 全部兄弟节点

repr() 方法可以将读取到的格式字符，比如换行符、制表符，转化为其相应的转义字符。

next_siblings和previous_siblings 属性

```
for sibling in soup.a.next_siblings:
    print(repr(sibling))
```

```
for sibling in soup.div.previous_siblings:
    print(repr(sibling))
```









# 二、BeautifulSoup 常用的API

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

