# selenium模块

# 1、selenium安装

```python
pip install selenium
```

# 2、selenium介绍

selenium的脚本可以控制浏览器进行操作，可以实现多个浏览器的调用，包括IE（7, 8, 9, 10, 11），Firefox，Safari，Google Chrome，Opera等。最常用的是 Firefox

浏览器驱动下载网址：

火狐浏览器：https://github.com/mozilla/geckodriver/releases 

谷歌浏览器：https://npm.taobao.org/mirrors/chromedriver/

# 3、selenium使用

## 3.1 常用的API

用 Selenium 操作浏览器主要通过 WebDriver 对象实现的，WebDriver对象提供了操作浏览器和访问 HTML 代码中数据的函数。

### 操作浏览器的函数如下：

> - refresh()：刷新网页
> - back()：回到上一个页面，必须有访问记录
> - forward()：进入下一个页面
> - close()：关闭窗口
> - quit()：结束浏览器的执行
> - get(url)：浏览URL 网址所指向的网页
> - maximize_window()：窗口最大化

### 访问 HTML 代码中数据的函数如下：

> - find_element_by_id：通过元素的id选择
>   - `<div id='bdy-inner'>test</div>`   `driver.find_element_by_id(‘bdy-inner’)`
> - find_element_by_name：通过元素的==name选择==，driver.find_element_by_name(‘password’)
>   - `input name="username type="text"`  `driver.find_element_by_name('password')`
> - find_element_by_xpath：通过xpath选择
>   - `form id="loginForm"`  `driver.find_element_by_xpath(“//form[@id='loginForm']”)`
> - find_element_by_link_text：通过==连接文本选择==
>   - `<a href="continue.html">Continue</a>`	`driver.find_element_by_link_text('Continue')`
> - find_element_by_partial_link_text：通过链接的部分地址选择
>   - `<a href="continue.html">Continue</a>`	`driver.find_element_by_link_text('Conti')`
> - find_element_by_tag_name：通过标签的名称选择
>   - `<h1>Welcome</h1>`	`driver.find_element_by_tag_name(‘h1’)`
> - find_element_by_class_name：通过CSS中的classs属性选择
>   - `<p class="content"> Site content goes here.</p>`	`driver.find_element_by_class_name(“content”)`
> - find_element_by_css_selector：通过元素的==css选择器选择==
>   - ```<div class='bdy-inner'>test</div>```  `driver.find_element_by_css_selector('div.bdy-inner')`

注：其中xpath和css_selector是比较好的方法，一方面比较清晰，另一方面相对其他方法定位元素比较准确。

​		查找所有元素使用elements

<font color='red' size =6>重要：</font>

新版本调用方法

```
from selenium.webdriver.common.by import By

driver.find_element(by=By.ID,value='BIZ_hq_historySearch')
```

### 常用属性：

> - current_url：获取当前页面的网址
> - page_source：返回当前页面的 HTML 代码
> - title：获取当前 HTML 页面的 title 属性值
> - text：返回标签中的文本内容
> - click：点击
> - send_keys：输入操作
> - get_attribute：获取元素的属性值

### 元素状态

> - 存在
> - 可见
> - 可用
> - 不可用

## 3.2 等待

### sleep - 强制等待

```
time.sleep(2)
```

### implicitly_wait - 隐形等待

一次会话中只需要调用一次，一般放在会话开始

场景：

- 等待元素被找着
- 等待命令被执行完成

```
driver.implicitly_wait(15)
```

## 3.3 使用设置

```python
# 设置浏览器驱动
driver_service = Service(executable_path=
                         r'D:\数据库\2022年项目\TOOL\火狐浏览器驱动-win64\geckodriver.exe')
# 把上述地址改成你电脑中geckodriver.exe程序的地址
driver = webdriver.Firefox(firefox_profile=fp,service=driver_service)
```

# 4、web自动化

## 4.1 8种定位方式

1类：根据元素的单一属性来定位,可能有多个值，定位只能选择一个

- id
- name
- class_name
- tag_name
- a链接元素 
-  文本

2类：组合元素的特征和关系来定位

- xpath（Ctrl + F）

  - 绝对定位(完整xpath): /开头 父子路径顺序，网页结构可能会变化，不稳定，不推荐

  - 相对定位: 

    - //标签名[@属性=值] 	

      //input[@id=‘kw’]

    - 逻辑运算： and or  

      //标签名[@属性=值 and @属性=值] 

    - 文本内容

      //标签名[text()=值]

    - 层级定位

      //标签名[@属性=值] //标签名[@属性=值] 

- css_selector

## 4.2 DOM对象

 HTML DOM 是关于如何获取、修改、添加或删除 HTML 元素的标准

- 获取到html的元素、元素的属性
- 可以对html的元素、元素的属性进行添加、修改、删除操作
- 对事件的响应

document对象：整个文档对象

### 获取元素

> document.getElementByID
>
> document.getElementsByName
>
> document.getElementsByTagName
>
> document.getElementsByClassName

### 获取元素属性

> ele.属性名称
>
> ele.getAttribute(属性名称)

### 修改元素的值

> ele.属性名称 = 值
>
> ele.setAttribute(属性名称)







# 案例：

## 爬虫案例

```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.firefox.service import Service

# 创建 Firefox 浏览器使用的Webdriver对象
driver_service = Service(executable_path=
                         r'D:\数据库\2022年项目\TOOL\火狐浏览器驱动-win64\geckodriver.exe')
# 把上述地址改成你电脑中geckodriver.exe程序的地址
driver = webdriver.Firefox(service=driver_service)

url = 'http://q.stock.sohu.com/cn/{0}/lshq.shtml'
strURL = url.format('600519')
print("请求的URL：", strURL)

# 发送请求
driver.get(strURL)

# 通过id找到表格标签对象， <table class="tableQ" id="BIZ_hq_historySearch">
tableElement = driver.find_element(by=By.ID, value='BIZ_hq_historySearch')

#  通过标签名找出表格中所有tr元素
trList = tableElement.find_elements(by=By.TAG_NAME, value='tr')

#  保存数据列表
datas = []
for index, tr in enumerate(trList):
    # 跳过表格前四行数据
    if index < 4:
        continue
    # 找到tr下面所有的元素
    tds = tr.find_elements(by=By.TAG_NAME, value='td')

    # 保存一行数据的字典对象
    row = dict()

    # 日期
    row['Date'] = tds[0].text
    # 开盘价
    row['Open'] = float(tds[1].text.replace(',', ''))
    # 收盘价
    row['Close'] = float(tds[2].text.replace(',', ''))
    # 最低价
    row['Low'] = float(tds[5].text.replace(',', ''))
    # 最高价
    row['High'] = float(tds[6].text.replace(',', ''))
    # 成交量
    row['Volume'] = float(tds[7].text.replace(',', ''))
    datas.append(row)

# 测试一下爬虫回来的数据
print(datas)

# 退出浏览器
driver.quit()
```

