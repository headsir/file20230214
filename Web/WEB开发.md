[TOC]

【web开发全家桶】https://www.bilibili.com/video/BV1rT4y1v7uQ?p=5&vd_source=d6c3edd9a4f6205095ccfba6b2a61eec

目的：开发一个平台（网站）

- 前端开发：HTML、CSS、JavaScript
- Web框架：接收请求并处理

- Mysql数据库：存储数据

快速上手：
	基于Flask Web框架快速搭建一个网站
深入学习：
	基于Django框架（主要）# 

# 一、前端

## 1 前端开发

### 1.1 快速开发网站（Flask）

- 规定有些文件必须放到指定文件夹
- 新创建一个页面
  - 函数
  - HTML文件

```markdown
pipenv install flask
```

```python
from flask import Flask

app = Flask(__name__)

# 创建了网址 /show/info 和函数index 的对应关系
# 以后用户在浏览器上访问 /show/info ,网站自动执行 index
@app.route("/show/info" , methods=["GET", "POST"])
def index():
    return "中国联通"

if __name__ == '__main__':
    app.run()
```

Flask框架为了方便写标签，支持将字符串写入文件中。

```python
from flask import Flask,render_template

app = Flask(__name__)

@app.route("/show/info")
def index():
    # Flask 内部会自动打开这个文件，并读取内容，将内容给用户返回
    # 默认：去当前项目目录的templates文件夹中找。
    return render_template("index.html")

if __name__ == '__main__':
    app.run(debug=True)
```

### 1.2 HTML标签

#### 1.2.1 编码

```html
<meta charset="UTF-8">
```

#### 1.2.2 网站主题

```html
<title>Title</title>
```

#### 1.2.3 标题

HTML 标题（Heading）是通过<h1> - <h6> 标签来定义的。

```html
    <h1 >一级标题</h1>
    <h2 >二级标题</h2>
    <h3 >三级标题</h3>
    <h4 >四级标题</h4>
    <h5 >五级标题</h5>
    <h6 >六级标题</h6>
```

![image-20230705234448725](imge/WEB开发.assets/image-20230705234448725.png)

#### 1.2.4 div和span

```html
<div>内容</div>
<span>as</span>
```

- div,占一整行【块级标签】
- span,自己多大占多少【行内标签，内联标签】

#### 1.2.5 超链接

a【行内标签】

```html
<!-- 跳转到其他网站 -->
<a href="https://www.baidu.com">点击跳转</a>

<!-- 跳转到自己网站的其他网站 -->
<a href="/get/news">点击跳转到http://127.0.0.1:5000/get/news</a>
```

```html
<!-- 当前页面打开-->
<a href="https://image.baidu.com/search/index"">

<!-- 新的Tab页面打开-->
<a href="https://image.baidu.com/search/index" target="_blank">
```

#### 1.2.6 图片

img【行内标签】

- 本地图片

 	Flask框架要求图片放置在：static目录

```html
<img src="/static/images/美女01.png"/>
```

- 其它来源图片（可能无法正常显示，为防止盗用【已加密】）

```
<img src="https://img1.baidu.com/it/u=2407290741,4013823944&fm=253&fmt=auto&app=138&f=JPEG?w=4&h=5" />
```

设置图片高度和宽度

```
<img  src="/static/images/美女01.png" style="width: 300px" />
```

#### 1.2.7 列表

- 无序列表

```html
<ul>
    <li>中国移动</li>
    <li>中国联通</li>
    <li>中国电信</li>
    <li>中国广电</li>
</ul>
```

![image-20230706004110225](imge/WEB开发.assets/image-20230706004110225.png)

- 有序列表

```html
<ol>
    <li>中国移动</li>
    <li>中国联通</li>
    <li>中国电信</li>
    <li>中国广电</li>
</ol>
```



![image-20230706004244599](imge/WEB开发.assets/image-20230706004244599.png)

#### 1.2.8 表格

```html
<table  border="1">表格
    <thead>
        <tr> <th style="border: 1px solid gray">ID</th>	<th>姓名</th>	<th>年龄</th> </tr>
    </thead>
    <tbody>
        <tr> <td style="border: 1px solid gray">10</td> <td>张三</td> <td>10</td> </tr>
        <tr> <td>11</td> <td>李四</td> <td>10</td> </tr>
        <tr> <td>12</td> <td>王五</td> <td>10</td> </tr>
        <tr> <td>13</td> <td>王麻子</td> <td>10</td> </tr>
        <tr> <td>14</td> <td>张器</td> <td>10</td> </tr>
    </tbody>
</table>
```

![image-20230706010403130](imge/WEB开发.assets/image-20230706010403130.png)

#### 1.2.9 input系列

```html
<!-- 文本输入-->
<input type="text"/>
<!--密码输入-->
<input type="password"/>
<!-- 文件选择-->
<input type="file"/>
<!-- 单选框-->
<input type="radio" name="n1"/>男
<input type="radio" name="n1"/>女
<!-- 复选框-->
<input type="checkbox" />乒乓球
<input type="checkbox"/>篮球
<--普通按钮-->
<input type="button" value="提交"/>
<--提交表单按钮,POST请求-->
<input type="submit" value="提交2"/>
```

#### 2.4.10 下拉框

```html
<!--单选下拉框-->
<select>
    <option>北京</option>
    <option>上海</option>
    <option>深圳</option>
</select>

<!--多选下拉框-->
<select multiple>
    <option>北京</option>
    <option>上海</option>
    <option>深圳</option>
</select>
```

#### 2.4.11 多行文本

```html
<!--默认3行-->
<textarea rows="3"></textarea>
```

#### 2.4.12 form表单和提交

- form标签
  - 提交方式(post/get)：`method="get" ` 
  - 提交地址：`action="xxx/xxx/xx/xx"`
  - 提交按钮：`<input type="submit" value="submit按钮">`

- 数据标签
  - 数据标签必须有`name`属性

- 后台数据获取
  - get方式：`request.args`
  - post方式：`request.form`
    - 单值获取：`request.form.get("username")`
    - 多值获取：`request.form.getlist("hobby")`

#### 案例：用户注册

```
<body>
<h1>用户注册</h1>
<!--<form method="get"  action="/do/reg">-->
<form method="post" action="/register">
    <div>
        用户名：<input type="text" name="username"/>
    </div>
    <div>
        密码：<input type="password" name="password"/>
    </div>
    <div>
        性别：
        <input type="radio" name="gender" value="1"/>男
        <input type="radio" name="gender" value="2"/>女
    </div>
    <div>
        爱好：
        <input type="checkbox" name="hobby" value="篮球"/>篮球
        <input type="checkbox" name="hobby" value="乒乓球"/>乒乓球
        <input type="checkbox" name="hobby" value="羽毛球"/>羽毛球
        <input type="checkbox" name="hobby" value="足球"/>足球
    </div>
    <div>
        城市：
        <select name="city">
            <option value="bj">北京</option>
            <option value="sh">上海</option>
            <option value="sz">深圳</option>
        </select>
    </div>
    <div>
        擅长领域：
        <select name="skill" multiple>
            <option value="游戏">游戏</option>
            <option value="睡觉">睡觉</option>
            <option value="刷抖音">刷抖音</option>
            <option value="吃饭">吃饭</option>
        </select>
    </div>
    <div>
        备注：<textarea name="more"></textarea>
    </div>
    <div>
        <input type="button" value="button按钮">
        <input type="submit" value="submit按钮">
    </div>
    <div>
        <span>选择文件框</span>
        <input name="file" type="file">
    </div>
</form>
</body>
```

![image-20230706225133549](imge/WEB开发.assets/image-20230706225133549.png)

#### 案例：用户登录

```
<body>
<h1>用户登录</h1>
<form method="post" action="/login">
    用户名：<input type="text" name ="username"/>
    密码：<input type="password" name ="password"/>
    <input type="submit" value="登录"/>
</form>
</body>
```

### 1.3 CSS样式

#### 1.3.1 快速了解

```
<img src="/static/images/美女01.png"  style="height:10px"/>

<div style="color: red;">中国联通</div>
```

#### 1.3.2 CSS应用方式

- 在标签上

  ```
  <img src="/static/images/美女01.png"  style="height:10px"/>
  
  <div style="color: red;">中国联通</div>
  ```

- 样式选择器（在head标签中写style标签）

  ```
  <style>
      .c1{
      	color: red;
      }
  </style>
      
  <h1 class="c1">用户登录</h1>
  ```

- CSS文件（写到文件中）

  ```
  <link rel="stylesheet" href="common.css"/>
  ```

#### 1.3.3 CSS选择器

- ID选择器

  ```html
  <!--ID选择器 -->
  #c2{
      height: 10px;
  }
  
  <div id="c2"></div>
  ```

- 类选择器

  ```html
  <!--类选择器 -->
  .c1{
  color: red;
  }
  <div class="c2"></div>
  ```

- 标签选择器

  ```html
  <!--标签选择器 -->
  div{
  color: green;
  }
  <div>xxxx</div>
  ```

- 属性选择器

  ```html
  input[type='text'] {
  	border: 1px solid red;
  }
  .v1[xx="456"]{
  	color: gold;
  }
  ```

  ```html
  <input type="text">
  <input type="password">
  
  <div class="v1" xx="123">e</div>
  <div class="v1" xx="456">d</div>
  <div class="v1" xx="789">f</div>
  ```

- 后代选择器

  ```html
  <!--每一代-->
  .yy li{
  	color: blue;
  }
  
  <!--第一代-->
  .yy > a{
  	color: blue;
  }
  ```

  ```html
  <div class="yy">
      <a>百度</a>
      <div>
          <a >谷歌</a>
      </div>
      <ul>
          <li>美国</li>
          <li>日报</li>
          <li>安国</li>
      </ul>
  </div>
  ```

  多个和覆盖:属性名相同后面覆盖前面，不同共同作用，不覆盖可添加`！important`

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
      <style>
          .c1 {
              color: red !important;
              border: 1px solid red;
          }
          .c2 {
              font-size: 28px;
              color: gold;
          }
      </style>
  </head>
  <body>
  <div class="c1 c2">中国联通</div>
  </body>
  </html>
  ```

#### 1.3.4 样式

##### 高度和宽度

注意事项

- 宽度支持百分比
- 行内标签：默认无效
- 块级标签：默认有效

```html
<style>
    .c1 {
        height: 300px;
        width: 500px;
    }
</style>
```

##### 块级和行内标签

- 块级

- 行内

- css样式：转换 `display:inline-block`

  块级、行内互相转换

  ```
  <div style="display:inline">中国</div>
  <span style="display: block">联通</span>
  ```

##### 字体和颜色

- 颜色
- 大小
- 加粗
- 字体

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1 {
            color: #00FF7F;
            font-size: 58px;
            font-weight: 600;
            font-family: emoji,楷体;
        }
    </style>
</head>
<body>
<div class="c1">中国联通</div>
<div>中国移动</div>
</body>
</html>
```

##### 文字对齐方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1 {
            height: 59px;
            width: 300px;
            border: 1px solid red;

            text-align: center; /* 水平方向居中 */
            line-height: 59px; /* 垂直方向居中 */
        }
    </style>

</head>
<body>
    <div class="c1">郭智</div>
</body>
</html>
```





# 二、Mysql

# 三、Django