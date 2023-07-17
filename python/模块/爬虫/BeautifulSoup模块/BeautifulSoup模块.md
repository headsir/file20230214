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

## 5.9 前后节点

在所有节点，不分层次

next_element 和previous_element 属性

## 5.10 所有前后节点

next_elements 和previous_elements 属性

```python
for element in soup.div.next_elements:
    print(repr(element))
```

# 六、搜索文档树

## 6.1 find_all( ）

搜索当前 tag 的所有 tag 子节点，并判断是否符合过滤器的条件

参数：name , attrs , recursive , text , **kwargs

### 6.1.1 name 参数

name 参数可以查找所有名字为 name 的 tag, 字符串对象会被自动忽略掉

- 传字符串

  最简单的过滤器是字符串。在搜索方法中传入一个字符串参数，Beautiful Soup 会查找与字符串完整匹配的内容。

  下面的例子用于查找文档中所有的 img 标签

  ```python
  soup.find_all("img")
  ```

- 传正则表达式

  如果传入正则表达式作为参数，Beautiful Soup 会通过正则表达式的 match () 来匹配内容。

  下面例子中找出所有以 b 开头的标签，这表示 b 开头标签都应该被找到

  ```python
  import re
  for tag in soup.find_all(re.compile("^b")):
      print(tag.name)
  ```

- 传列表

  如果传入列表参数，Beautiful Soup 会将与列表中任一元素匹配的内容返回。

  下面代码找到文档中所有 a 标签和 b 标签

  ```python
  soup.find_all(["b", "img"])
  ```

- 传 True

  True 可以匹配任何值

  下面代码查找到所有的 tag, 但是不会返回字符串节点

  ```
  for tag in soup.find_all(True):
      print(tag.name)
  ```

- 传方法

  如果没有合适过滤器，那么还可以定义一个方法，方法只接受一个元素参数 , 如果这个方法返回 True 表示当前元素匹配并且被找到，如果不是则反回 False 

  下面方法校验了当前元素，如果包含 class 属性却不包含 id 属性，那么将返回 True

  自定义方法:

  校验当前元素,如果包含 `class` 属性却不包含 `id` 属性,那么将返回 `True`

  ```
  def has_class_but_no_id(tag):
      return tag.has_attr('class') and not tag.has_attr('id')
  ```

  ```
  [tag.attrs for tag in soup.find_all(has_class_but_no_id)]
  ```

### 6.1.2 keyword 参数

注意：如果一个指定名字的参数不是搜索内置的参数名，搜索时会把该参数当作指定名字 tag 的属性来搜索，如果包含一个名字为 id 的参数，Beautiful Soup 会搜索每个 tag 的”id” 属性

```
soup.find_all(id="sugarea")
```

- 如果传入 href 参数，Beautiful Soup 会搜索每个 tag 的”href” 属性

```
soup.find_all(href=re.compile("id=1771629833664382922"))

[<a href="https://baijiahao.baidu.com/s?id=1771629833664382922" mon="a=9" target="_blank">华为加入“百模大战”，好戏才刚刚开始！</a>]
```

- 使用多个指定名字的参数可以同时过滤 tag 的多个属性

```
soup.find_all(href=re.compile("baidu"),style="top: 29px;")
```

-  class 过滤， class 是 python 的关键词，加个下划线

```
soup.find_all(href=re.compile("baidu"),class_="img")
```

- 有些 tag 属性在搜索不能使用，比如 HTML5 中的 data- 属性

  可以通过 find_all () 方法的 attrs 参数定义一个字典参数来搜索包含特殊属性的 tag

  根据属性查找标签`soup.select("*[data-control]")`

```
soup.find_all(attrs={"data-control": "pane-news"})

[<a data-control="pane-news" href="javascript:void(0);">热点要闻</a>]
```

### 6.1.2 text 参数

通过 text 参数可以搜搜文档中的字符串内容。与 name 参数的可选值一样，text 参数接受字符串，正则表达式，列表，True

```
soup.find_all(text=" Elsie ")
soup.find_all(text=["百度新闻——海量中文资讯平台", " Elsie ", "我的主页"])
soup.find_all(text=re.compile("新闻$"))
```

### 6.1.3 limit 参数

limit 参数限制返回结果的数量。效果与 SQL 中的 limit 关键字类似

```
soup.find_all("a", limit=4)
```

### 6.1.4 recursive 参数

调用 tag 的 find_all () 方法时，Beautiful Soup 会检索当前 tag 的所有子孙节点，如果只想搜索 tag 的直接子节点，可以使用参数 recursive=False 

```
soup.body.find_all("a",recursive=False)
```

## 6.2 find()

( name , attrs , recursive , text , \*\*kwargs )  

它与 find_all () 方法唯一的区别是 find_all () 方法的返回结果是值包含一个元素的列表，而 find () 方法直接返回结果 

## 6.3 find_parents() 和find_parent()

 find_all () 和 find () 只搜索当前节点的所有子节点，孙子节点等. find_parents () 和 find_parent () 用来搜索当前节点的父辈节点，搜索方法与普通 tag 的搜索方法相同，搜索文档搜索文档包含的内容

## 6.4 find_next_siblings() 和find_next_sibling()

这 2 个方法通过 .next_siblings 属性对当 tag 的所有后面解析的兄弟 tag  节点进行迭代，

- find_next_siblings () 方法返回所有符合条件的后面的兄弟节点

- find_next_sibling ()  只返回符合条件的后面的第一个 tag 节点  

## 6.5 find_previous_siblings() 和find_previous_sibling()

这 2 个方法通过 .previous_siblings 属性对当前 tag 的前面解析的兄弟 tag  节点进行迭代

- find_previous_siblings ()  方法返回所有符合条件的前面的兄弟节点

- find_previous_sibling () 方法返回第一个符合条件的前面的兄弟节点 

## 6.6 find_all_next() 和find_next()

 这 2 个方法通过 .next_elements 属性对当前 tag 的之后的 tag 和字符串进行迭代，

- find_all_next () 方法返回所有符合条件的节点，

- find_next () 方法返回第一个符合条件的节点

## 6.7 find_all_previous () 和 find_previous ()

这 2 个方法通过 .previous_elements 属性对当前节点前面的 tag 和字符串进行迭代

- find_all_previous () 方法返回所有符合条件的节点

- find_previous () 方法返回第一个符合条件的节点 

注：以上（2）（3）（4）（5）（6）（7）方法参数用法与 find_all () 完全相同，原理均类似，在此不再赘述。           

## 6.8 CSS 选择器

soup.select()，返回类型是 list

### 6.8.1 通过标签名查找

```
soup.select('a')
```

### 6.8.2 通过类名查找

```
soup.select('.img-link')

[<a class="img-link img-link1" href="https://cn.china.cn/" target="_blank"> </a>,
 <a class="img-link img-link2" href="http://cyberpolice.mps.gov.cn/wfjb/" target="_blank"> </a>,
 <a class="img-link img-link3" href="http://www.bjjubao.org/" target="_blank"> </a>]
```

### 6.8.3 通过 id 名查找

```
soup.select('#app_tooltip')

[<a href="javascript:void(0)" id="app_tooltip" mon="target=appLink" style="margin-right:20px;">百度新闻客户端
 <div id="app_tooltip_qrcode">
 <img src="%E7%99%BE%E5%BA%A6%E6%96%B0%E9%97%BB%E2%80%94%E2%80%94%E6%B5%B7%E9%87%8F%E4%B8%AD%E6%96%87%E8%B5%84%E8%AE%AF%E5%B9%B3%E5%8F%B0_files/newErweima_9fa03e0.png"/>
 </div>
 </a>]
```

### 6.8.4 组合查找

组合查找即和写 class 文件时，标签名与类名、id 名进行的组合原理是一样的，例如查找 body 标签中，id 等于 app_tooltip 的内容，二者需要用空格分开

```
soup.select('body #app_tooltip')
```

直接子标签查找

```
soup.select("head > title")
```

### 6.8.5 属性查找

​		查找时还可以加入属性元素，属性需要用中括号括起来，注意属性和标签属于同一节点，所以中间不能加空格，否则会无法匹配到。

```
soup.select('a[class="tab-login tab-enter-recommend"]')
```

```
soup.select('a[href="https://news.baidu.com/guoji"]')
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

