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
> - window_handles：  所有窗口句柄
> - current_window_handle ：当前窗口句柄
> - switch_to.alert：切换到弹窗
> - switch_to.window：切换窗口

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
> - get_screenshot_as_file()  # 保存浏览器截图，以.png保存
> - tag_name：元素名称
> - rect：元素大小、位置
> - screenshot()：单个元素截图
> - clear：清空内容
> - is_selected：是否被选中，返回True
> - is_enabled：是否可用
> - is_displayed：是否显示
> - value_of_css_property：css属性值

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

### 显性等待

根据条件等待

WebDriverWait(driver（会话对象）,等待总时长,判断间隔时长（默认0.5秒）)

```
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# 条件：指定的元素可见
ele_locator = (By.XPATH,'//div[@id="u1"]//a[@name="tj_login"]')
EC.visibility_of_element_located(ele_locator)  # 条件

wait = WebDriverWait(driver,15)  # 对象设置
wait.until(EC.visibility_of_element_located(ele_locator))  # 条件判断
```

## 3.3 窗口切换

- 等到所有的窗口：wins = driver.window_handles  返回的是窗口的句柄列表
- 新窗口： new_win = wins[-1]  最后一个窗口
- 切换窗口： driver.switch_to.window(new_win)
- 等待新窗口出现

## 3.4 鼠标操作

ActionChains类

鼠标操作（参数为元素对象）：

> 1. 点击 - click
> 2. 右键 - context_click
> 3. 双击 - double_click
> 4. 悬浮 - move_to_element
> 5. 按下左键 - click_and_hold
> 6. 释放按键 - release
> 7. 拖拽 - drag_and_drop
> 8. 等待 - pause
> 9. 输入 - send_keys

执行鼠标操作：

perform()

操作实例：

```
# 导入模块
from selenium.webdriver.common.action_chains import ActionChains

# 实例化ActionChains类
ac = ActionChains(driver)
# 找到要操作的元素，再调用鼠标操作
ele = driver.find_element(by=By.XPATH,value="")
# 调用perform执行鼠标操作
ac.move_to_element(ele).perform()
```

键盘的操作

【参考】https://www.bilibili.com/video/BV1BS4y1g74o?p=17&vd_source=d6c3edd9a4f6205095ccfba6b2a61eec

```
from selenium.webdriver.common.keys import Keys
```

## 3.5 使用设置

```python
import ssl
# 忽略报错
ssl._create_default_https_context = ssl._create_unverified_context

# 设置浏览器驱动
driver_service = Service(executable_path=
                         r'D:\数据库\2022年项目\TOOL\火狐浏览器驱动-win64\geckodriver.exe')
# 把上述地址改成你电脑中geckodriver.exe程序的地址
driver = webdriver.Firefox(service=driver_service)

# 添加cookie
driver.add_cookie({"name":"","value":"值"})

# 给元素加框
d='arguments[0].style.border="3px dashed #FFCCFF"'#边框格式 颜色
driver.execute_script("边框格式"," 元素") #给选择元素加边框

debugger()
```

## 3.6 下拉列表的操作

> 1. 根据值选择：select_by_value
> 2. 根据索引选择：select_by_index
> 3. 根据文本选择：select_by_visible_text
> 4. 反选使用：deselect
> 5. 反选所有：deselect_all
> 6. 所有选项：options
> 7. 所有选中选项：all_selected_options
> 8. 第一个选择选项：first_selected_option

```
from selenium.webdriver.support.select import Select

```

## 3.7 页面弹窗操作

> 页面弹出种类：
>
> - alert：用来提示
>
> - confirm：用来确认
>
> - prompt：输入内容
>
>   方法：
>
>   - accept：接受
>   - dismiss：取消
>   - text：显示文本
>   - send_keys：输入内容

## 3.8 iframe处理

切换页面 driver.switch_to.frame(iframe id)

```
driver.switch_to.frame("page-mainIframereport")
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

  - xpath调试

    控制台 $x(语法)

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

## 4.3 文件上传

点击上传：input标签不能点击，可以输入文件绝对路径

拖拽上传（drop方法）：upload_by_drop(文件绝对路径)

## 4.4 文件下载

设置下载目录：set_download_path(目录)



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

