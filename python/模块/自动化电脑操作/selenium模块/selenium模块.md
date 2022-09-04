# selenium模块

# 1、selenium安装

```python
pip install selenium
```

# 2、selenium介绍

selenium的脚本可以控制浏览器进行操作，可以实现多个浏览器的调用，包括IE（7, 8, 9, 10, 11），Firefox，Safari，Google Chrome，Opera等。最常用的是 Firefox

# 3、selenium使用

## 3.1 常用的API

用 Selenium 操作浏览器主要通过 WebDriver 对象实现的，WebDriver对象提供了操作浏览器和访问 HTML 代码中数据的函数。

### 操作浏览器的函数如下：

> - refresh()：刷新网页
> - back()：回到上一个页面
> - forward()：进入下一个页面
> - close()：关闭窗口
> - quit()：结束浏览器的执行
> - get(url)：浏览URL 网址所指向的网页

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

### 常用属性：

> - current_url：获取当前页面的网址
> - page_source：返回当前页面的 HTML 代码
> - title：获取当前 HTML 页面的 title 属性值
> - text：返回标签中的文本内容





- 打开浏览器和一个网页

  ```python
  from selenium import webdriver
  driver = webdriver.Firefox()
  driver.get("http://www.santostang.com/2018/07/04/hello-world/")
  ```

  运行之后，发现程序报错，错误为：
   selenium.common.exceptions.WebDriverException:Message: ‘geckodriver’ executable needs to be in PATH.
   在selenium之前的版本中，这样做是不会报错的，但是Selenium新版无法运行。

  我们要下载geckodriver，可以到https://github.com/mozilla/geckodriver/releases  下载相应操作系统的geckodriver，这是一个压缩文件，解压后可以放在桌面，如C:\Users\santostang\Desktop\geckodriver.exe。最后的代码如下：

  ```python
  from selenium import webdriver
  from selenium.webdriver.firefox.service import Service
  driver_service = Service(executable_path=r'D:\数据库\2022年项目\爬虫系列\selenium 浏览器驱动\火狐驱动\geckodriver.exe')
  # 把上述地址改成你电脑中geckodriver.exe程序的地址
  driver = webdriver.Firefox(service=driver_service)
  driver.get("http://www.santostang.com/2018/07/04/hello-world/")
  ```

#### 