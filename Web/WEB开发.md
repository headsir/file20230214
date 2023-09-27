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

注释

- HTML注释

  ```html
  <!-- 注释内容-->
  ```

- CSS注释

  ```css
  /* 注释内容*/
  ```

- JavaScript注释

  ```js
  // 注释内容
  
  /* 注释内容 */
  ```

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

##### 浮动

注意：浮动会使标签脱离文档流。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .item {
            /*左边浮动*/
            float: left;
            /*宽度*/
            width: 200px;
            /*高度*/
            height: 170px;
            /*红色边框*/
            border: 1px solid red;
        }
    </style>
</head>
<body>
<div>
    <span>左边</span>
    <!--    右边浮动-->
    <span style="float:right">右边</span>
</div>
<!--添加蓝色底色-->
<div style="background-color: dodgerblue">
    <div class="item">一</div>
    <div class="item">二</div>
    <div class="item">三</div>
    <div class="item">四</div>
    <div class="item">五</div>
    <div class="item">六</div>
    <!-- 解除浮动脱离文档流-->
    <div style="clear: both"></div>
</div>
<div>您好</div>
</body>
</html>
```

![image-20230707152448562](imge/WEB开发.assets/image-20230707152448562.png)

##### 内边距

内边距，标签内部距离

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .outer {
            border: 1px solid red;
            height: 200px;
            width: 200px;
            /*内边距：标签内部距离*/
            /*padding-top: 20px;*/
            /*padding-left: 20px;*/
            /*padding-right: 20px;*/
            /*padding-bottom: 20px;*/
            /*代表上下左右*/
            /*padding: 20px;*/
            /*分别代表 上 右 下 左（顺时针）*/
            padding: 20px 10px 5px 3px;
        }
    </style>
</head>
<body>
<div class="outer">
    <div style="background-color: gold">听妈妈的话</div>
    <div>小朋友是否下水</div>
</div>
</body>
</html>
```

##### 外边距

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div style="height: 200px;background-color: dodgerblue;"></div>
<!--外边距：外部距离 margin-top: 20px-->
<div style="background-color: red;height: 100px;margin-top: 20px;"></div>
</body>
</html>
```

##### 禁止复制粘贴

在html中，可以利用touch-callout和user-select属性来属性禁止复制粘贴功能，只需要设置“user-select:none;-webkit-touch-callout:none;”样式即可

#### 1.3.5 CSS案例

##### 小米商城顶部

- body标签，默认有一个边距

  ```css
  /*去除边距*/
  body {
      /*外边距为0*/
      margin: 0;
  }
  ```

- 内容居中

  - 文本居中

    ```css
    <div style="
    /*外边距*/
    margin-top: 30px;
    /*标签宽度*/
    width:800px;
    /*文本居中*/
    text-align: center;
    /*添加红色边框*/
    border: 1px solid red
    ">文本居中</div>
    ```

    ![image-20230707180554815](imge/WEB开发.assets/image-20230707180554815.png)

  - 区域局中

    自己有宽度，左右自动

    ```CSS
    .container {
        width: 980px;
        /*外边距：上下0 左右 自动*/
        margin: 0 auto;
    }
    ```

![image-20230707174955698](imge/WEB开发.assets/image-20230707174955698.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>小米商城 - Xiaomi 13、Redmi K60、MIX FOLD 2，小米电视官方网站</title>
    <style>
        body {
            /*外边距为0*/
            margin: 0;
        }

        .header {
            /*背景色：#333*/
            background-color: #333;
        }

        .container {
            width: 980px;
            /*外边距：上下0 左右 自动*/
            margin: 0 auto;
        }

        .header .menu {
            /*设置标签浮动*/
            float: left;
            /*标签颜色*/
            color: white;
        }

        .header .account {
            float: right;
            color: white;
        }

        .header a {
            color: #b0b0b0;
            /*标签高度*/
            line-height: 40px;
            display: inline-block;
            font-size: 12px;
            margin-right: 10px;
        }
    </style>
</head>
<body>

<div class="header">
    <div class="container">
        <div class="menu">
            <a>小米商城</a>
            <a>MIUI</a>
            <a>云服务</a>
            <a>有品</a>
            <a>开放平台</a>
        </div>
        <div class="account">
            <a href="">登录</a>
            <a href="">注册</a>
            <a href="">消息</a>
            <a href="">通知</a>
        </div>
        <!--解除浮动-->
        <div style="clear: both"></div>
    </div>
</div>
</body>
</html>
```

##### 小米商城二级菜单

```
a标签，默认有下划线
/*取消下划线*/
text-decoration: none;

鼠标放上面变格式
/*鼠标放到上面显示样式设置*/
 a:hover {
color: #ff6788;
}
```

1、 划分区域

2、搭建框架

3、设置样式

模板：

![image-20230709160423123](imge/WEB开发.assets/image-20230709160423123.png)

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body {
            margin: 0;
        }

        .sub-header {
            height: 100px;
            /*background-color: #b0b0b0;*/
        }

        .container {
            width: 1128px;
            margin: 0 auto;
            /*border: 1px solid red;*/
        }

        .sub-header .height {
            height: 100px
        }

        .sub-header .log {
            width: 234px;
            float: left;
            /*border: 1px solid red;*/
        }

        .sub-header .log a {
            margin-top: 22px;
            display: inline-block;
        }

        .sub-header .log a img {
            height: 56px;
            width: 56px;
        }

        .sub-header .menu-list {
            float: left;
            line-height: 100px;
            /*border: 1px solid red;*/
        }

        .sub-header .menu-list a {
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            /*取消下划线*/
            text-decoration: none;
        }
        /*鼠标放到上面显示样式设置*/
        .sub-header .menu-list a:hover{
            color:#ff6788;
        }

        .sub-header .search {
            float: right;
            display: inline-block;
            line-height: 100px;
            /*border: 1px solid red;*/

        }
    </style>
</head>
<body>
<div class="sub-header">
    <div class="container">
        <div class="log height">
            <a href="https://www.mi.com">
                <img src="images/log.png" alt="">
            </a>
        </div>
        <div class="menu-list height">
            <a href="https://www.mi.com">Xiaomi手机</a>
            <a href="https://www.mi.com">Redmi手机</a>
            <a href="https://www.mi.com">电视</a>
            <a href="https://www.mi.com">笔记本</a>
            <a href="https://www.mi.com">平板</a>
            <a href="https://www.mi.com">家电</a>
            <a href="https://www.mi.com">路由器</a>
            <a href="https://www.mi.com">服务中心</a>
            <a href="https://www.mi.com">社区</a>
        </div>
        <div class="search height">搜索框暂时先不做</div>
        <div style="clear: both"></div>
    </div>
</div>

</body>
</html>
```

显示：

![image-20230709170231747](imge/WEB开发.assets/image-20230709170231747.png)

##### 小米商城推荐区域

透明度

```
/*透明度*/
opacity: 0.7;
```



模板：

![image-20230709172243714](imge/WEB开发.assets/image-20230709172243714.png)

代码：

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>小米商城 - Xiaomi 13、Redmi K60、MIX FOLD 2，小米电视官方网站</title>
    <style>
        body {
            /*外边距为0*/
            margin: 0;
        }

        img {
            width: 100%;
            height: 100%;
        }

        .left {
            float: left;
        }

        .header {
            /*背景色：#333*/
            background-color: #333;
        }

        .container {
            width: 1226px;
            /*外边距：上下0 左右 自动*/
            margin: 0 auto;
        }

        .header .menu {
            /*设置标签浮动*/
            float: left;
            /*标签颜色*/
            color: white;
        }

        .header .account {
            float: right;
            color: white;
        }

        .header a {
            color: #b0b0b0;
            /*标签高度*/
            line-height: 40px;
            /*转换 行、块*/
            display: inline-block;
            /*字体大小*/
            font-size: 12px;
            /*外边距*/
            margin-right: 10px;
            /*取消下划线*/
            text-decoration: none;

        }

        .header a:hover {
            color: white;
            /*取消下划线*/
            text-decoration: none;
        }

        .sub-header {
            height: 100px;
            /*background-color: #b0b0b0;*/
        }

        .sub-header .height {
            height: 100px
        }

        .sub-header .log {
            width: 234px;
            float: left;
            /*border: 1px solid red;*/
        }

        .sub-header .log a {
            margin-top: 22px;
            display: inline-block;
        }

        .sub-header .log a img {
            height: 56px;
            width: 56px;
        }

        .sub-header .menu-list {
            float: left;
            line-height: 100px;
            /*border: 1px solid red;*/
        }

        .sub-header .menu-list a {
            /*内边距,上下0 左右10px*/
            padding: 0 10px;
            display: inline-block;
            color: #333333;
            font-size: 16px;
            /*取消下划线*/
            text-decoration: none;
            /*border: 1px solid red;*/
        }

        /*鼠标放到上面显示样式设置*/
        .sub-header .menu-list a:hover {
            color: #ff6788;
        }

        .sub-header .search {
            float: right;
            display: inline-block;
            line-height: 100px;
            /*border: 1px solid red;*/

        }

        .slider .sd-img {
            /*width: 1226px;*/
            height: 460px;
        }

        .news {
            padding-top: 14px;
        }

        .news .channel {
            width: 228px;
            height: 164px;
            background-color: #5f5750;
            padding: 3px;
        }

        .news .channel .item {
            height: 86px;
            width: 76px;
            /*border: 1px solid royalblue;*/
            float: left;
            text-align: center;
        }

        .news .channel .item a {
            display: inline-block;
            font-size: 12px;
            padding-top: 18px;
            color: white;
            text-decoration: none;
            /*透明度*/
            opacity: 0.7;
        }

        .news .channel .item a:hover {
            opacity: 1;
        }

        .news .channel img {
            height: 24px;
            width: 24px;
            display: block;
            /*上0 左右自动 下4*/
            margin: 0 auto 4px;

        }

        .news .list-item {
            width: 316px;
            height: 170px;
        }

    </style>
</head>
<body>

<div class="header">
    <div class="container">
        <div class="menu">
            <a href="https://www.mi.com">小米商城</a>
            <a href="https://www.mi.com">MIUI</a>
            <a href="https://www.mi.com">云服务</a>
            <a href="https://www.mi.com">有品</a>
            <a href="https://www.mi.com">开放平台</a>
        </div>
        <div class="account">
            <a href="https://www.mi.com">登录</a>
            <a href="https://www.mi.com">注册</a>
            <a href="https://www.mi.com">消息</a>
            <a href="https://www.mi.com">通知</a>
        </div>
        <!--解除浮动-->
        <div style="clear: both"></div>
    </div>
</div>
<div class="sub-header">
    <div class="container">
        <div class="log height">
            <a href="https://www.mi.com">
                <img src="images/log.png" alt="">
            </a>
        </div>
        <div class="menu-list height">
            <a href="https://www.mi.com">Xiaomi手机</a>
            <a href="https://www.mi.com">Redmi手机</a>
            <a href="https://www.mi.com">电视</a>
            <a href="https://www.mi.com">笔记本</a>
            <a href="https://www.mi.com">平板</a>
            <a href="https://www.mi.com">家电</a>
            <a href="https://www.mi.com">路由器</a>
            <a href="https://www.mi.com">服务中心</a>
            <a href="https://www.mi.com">社区</a>
        </div>
        <div class="search height">搜索框暂时先不做</div>
        <div style="clear: both"></div>
    </div>
</div>
<div class="slider">
    <div class="container">
        <div class="sd-img">
            <img src="images/Note12T%20Pro.jpeg" alt="">
        </div>
    </div>

</div>
<div class="news">
    <div class="container">
        <div class="channel left">
            <div class="item">
                <a href="https://www.mi.com">
                    <img src="images/保障服务.png" alt="">
                    <span>保障服务</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com">
                    <img src="images/企业团购.png" alt="">
                    <span>企业团购</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com">
                    <img src="images/F码通道.png" alt="">
                    <span>F码通道</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com">
                    <img src="images/米粉卡.png" alt="">
                    <span>米粉卡</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com">
                    <img src="images/以旧换新.png" alt="">
                    <span>以旧换新</span>
                </a>
            </div>
            <div class="item">
                <a href="https://www.mi.com">
                    <img src="images/话费充值.png" alt="">
                    <span>话费充值</span>
                </a>
            </div>
            <!--            <div class="clear:both"></div>-->
        </div>
        <div class="list-item left" style="margin-left: 14px">
            <img src="images/Note12%20Turbo.jpg" alt="">
        </div>
        <div class="list-item left" style="margin-left: 15px">
            <img src="images/xlaoml 13系列.jpg" alt="">
        </div>
        <div class="list-item left" style="margin-left: 15px">
            <img src="images/K60.jpg" alt="">
        </div>
        <!--        <div class="clear: both"></div>-->
    </div>
</div>
</body>
</html>
```



显示：

![image-20230712234442438](imge/WEB开发.assets/image-20230712234442438.png)

#### 1.3.6 CSS知识点

##### hover(伪类)

```css
.download{
    /*隐藏*/
    display: none;
}

.app:hover .download{
    /*显示*/
    display: block;
}
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1 {
            color: red;
            font-size: 18px;
        }

        .c1:hover {
            color: green;
            font-size: 50px;
        }

        .c2 {
            height: 300px;
            width: 500px;
            border: 1px solid red;
        }

        .c2:hover {
            border: 5px solid green;
        }

        .download{
            /*隐藏*/
            display: none;
        }

        .app:hover .download{
            /*显示*/
            display: block;
        }
        .app:hover .title{
            color: red;
        }
    </style>
</head>
<body>
    <div class="c1">联通</div>
    <div class="c2">广西</div>
    
    <div class="app">
        <div class="title">下载AAP</div>
        <div class="download">
            <img src="images/APP二维码.png" alt="">
        </div>
    </div>
</body>
</html>
```

效果：

![20230713-214623](imge/WEB开发.assets/20230713-214623.gif)

##### after(伪类)

```css
/*标签尾部添加东西*/
.c1:after {
    content: "大帅哥";
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>

        /*标签尾部添加东西*/
        .c1:after {
            content: "大帅哥";
        }
        .clearfix{
            background-color: #ff6788;
        }
        .clearfix:after {
            /*标签后面填充字符为空*/
            content: "";
            /*div 变块级标签*/
            display: block;
            /*清除浮动*/
            clear: both;
        }
        .item{
            float: left;
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <div class="c1">武阳郡</div>
    <div class="clearfix">
        <div class="item">1</div>
        <div class="item">2</div>
        <div class="item">3</div>
        <div class="clear:both"></div>
    </div>
</body>
</html>
```

效果：

![image-20230713220832945](imge/WEB开发.assets/image-20230713220832945.png)

##### position（窗口位置）

- fixed 固定在窗口的某个位置

- > 设置相对位置 relative absolute

###### fixed

固定在窗口的某个位置

###### 案例：返回顶部

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <style>
    .back{
      /*固定在窗口的某个位置*/
      position: fixed;

      width: 68px;
      height: 60px;
      border: 1px solid red;

      /*右边*/
      right: 0;
      /*底部*/
      bottom:0;
    }
  </style>
</head>
<body>
<!--高度1000px,底色灰色-->
<div style="height: 1000px;background-color: #b0b0b0"></div>
  <div class="back">返回顶部</div>
</body>
</html>
```

效果：

![image-20230713223328786](imge/WEB开发.assets/image-20230713223328786.png)

###### 案例：对话框

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .mask{
            background-color: black;
            position: fixed;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            opacity: 0.7;
            z-index: 999;
        }
        .dialog {
            /*固定在窗口的某个位置*/
            position: fixed;

            width: 280px;
            height: 200px;
            background-color: white;

            left: 0;
            right: 0;
            margin: 0 auto;

            top: 200px;
            /*谁的值大谁在上面*/
            z-index: 1000;
        }
    </style>
</head>
<body>


<!--高度1000px,底色灰色-->
<div style="height: 1000px;">gggggggg</div>
<div class="dialog">对话框</div>
<div class="mask"></div>


</body>
</html>
```

###### relative和absolute

设置相对位置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .c1{
            height: 300px;
            width: 500px;
            border: 1px solid red;
            margin: 100px;

            position: relative;
        }

        .c1 .c2{
            height: 59px;
            width: 59px;
            background-color: #00FF7F;

            position: absolute;
            /*相对c1的位置*/
            right: 20px;
            bottom: 20px;
        }
    </style>
</head>
<body>
<div class="c1">
    <div class="c2"></div>
    <div class="c2"></div>
    <div class="c2"></div>
    <div class="c2"></div>
</div>
</body>
</html>
```

##### border（边框）

```css
/*线条宽度 实线 颜色*/
border: 1px solid red;

transparent 透明色
```

##### background-color（背景色）

```
background-color: #00FF7F;
```

参见上面案例

### 1.4 BootStrap 模板

#### 下载

官网：https://v3.bootcss.com/

![image-20230714162958760](imge/WEB开发.assets/image-20230714162958760.png)

#### 依赖

依赖JavaScript的类库，jQuery。

- 下载jQuery，在页面上应用jQuery

  ```
  https://releases.jquery.com/jquer
  ```

- 在页面上应用BootStrap的JavaScript类库

  ```
  <script src="static/js/jquery-3.6.0.min.js"></script>
  <script src="static/plugins/bootstrap-3.4.1/js/bootstrap.min.js"></script>
  ```

  



#### 使用

![image-20230714163507137](imge/WEB开发.assets/image-20230714163507137.png)

##### 1.4.1 CSS样式

###### 初识bootstrap

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--  开发版本  -->
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <!--  生产版本（开发版本压缩后的文件，占用空间小）  -->
<!--    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.min.css">-->
</head>
<body>
    <input type="button" value="提交" />
    <input type="button" value="提交" class="btn btn-primary"/>
    <input type="button" value="提交" class="btn btn-success" />
    <input type="button" value="提交" class="btn btn-danger" />
    <input type="button" value="提交" class="btn btn-danger btn-xs" />
</body>
</html>
```

效果：

![image-20230714164815300](imge/WEB开发.assets/image-20230714164815300.png)

###### 导航条

可以对模板里面的样式重写

```CSS
<style>
    .navbar {
        border-radius: 0;
    }
</style>
```

![image-20230714170816659](imge/WEB开发.assets/image-20230714170816659.png)

###### 栅格系统

把整体划分为12格

分类

- 响应式

  根据屏幕宽度，自动堆叠

  ```
  .col-sm-  	750px
  .col-md-    970px
  .col-lg-    1170px
  ```

  ```css
  <div class="col-lg-5" style="background-color: #00FF7F">5</div>
  <div class="col-lg-6" style="background-color: red">6</div>
  ```

- 非响应式

  总是水平排列
  
  ```html
  <!--    非响应式-->
  <div class="col-xs-4" style="background-color: #00FF7F">4</div>
  <div class="col-xs-5" style="background-color: red">4</div>
  ```
  
- 列偏移

  ```
  <div class="col-sm-offset-1 col-lg-5" style="background-color: #00FF7F">5</div>
  <div class="col-lg-6" style="background-color: red">6</div>
  ```

![image-20230714171255480](imge/WEB开发.assets/image-20230714171255480.png)

###### 案例：博客

![image-20230718225813191](imge/WEB开发.assets/image-20230718225813191.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--  开发环境-->
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .navbar {
            border-radius: 0;
        }

        .navbar-default {
            background-color: #c1e2b3;
            border-color: #e7e7e7;
        }
    </style>
</head>
<body>
<div>
    <!--    导航条-->
    <nav class="navbar navbar-default">
        <div class="container-fluid">
            <!-- Brand and toggle get grouped for better mobile display -->
            <div class="navbar-header">
                <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                        data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="#">博客</a>
            </div>

            <!-- Collect the nav links, forms, and other content for toggling -->
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav">
                    <li class="active"><a href="#">联通 <span class="sr-only">(current)</span></a></li>
                    <li><a href="#">广西</a></li>
                    <li class="dropdown">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                           aria-expanded="false">Dropdown <span class="caret"></span></a>
                        <ul class="dropdown-menu">
                            <li><a href="#">Action</a></li>
                            <li><a href="#">Another action</a></li>
                            <li><a href="#">Something else here</a></li>
                            <li role="separator" class="divider"></li>
                            <li><a href="#">Separated link</a></li>
                            <li role="separator" class="divider"></li>
                            <li><a href="#">One more separated link</a></li>
                        </ul>
                    </li>
                </ul>
                <form class="navbar-form navbar-left">
                    <div class="form-group">
                        <input type="text" class="form-control" placeholder="Search">
                    </div>
                    <button type="submit" class="btn btn-default">搜索</button>
                </form>
                <ul class="nav navbar-nav navbar-right">
                    <li><a href="#">注册</a></li>
                    <li><a href="#">登录</a></li>
                    <li class="dropdown">
                        <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
                           aria-expanded="false">Dropdown <span class="caret"></span></a>
                        <ul class="dropdown-menu">
                            <li><a href="#">Action</a></li>
                            <li><a href="#">Another action</a></li>
                            <li><a href="#">Something else here</a></li>
                            <li role="separator" class="divider"></li>
                            <li><a href="#">Separated link</a></li>
                        </ul>
                    </li>
                </ul>
            </div><!-- /.navbar-collapse -->
        </div><!-- /.container-fluid -->
    </nav>

    <!-- 栅格   -->
    <div class="container-fluid clearfix">
        <div class="col-sm-9">
            <!--            媒体对象-->
            <div class="media">
                <div class="media-left">
                    <a href="#">
                        <img class="media-object" data-src="holder.js/64x64" alt="64x64"
                             src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/PjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+PCEtLQpTb3VyY2UgVVJMOiBob2xkZXIuanMvNjR4NjQKQ3JlYXRlZCB3aXRoIEhvbGRlci5qcyAyLjYuMC4KTGVhcm4gbW9yZSBhdCBodHRwOi8vaG9sZGVyanMuY29tCihjKSAyMDEyLTIwMTUgSXZhbiBNYWxvcGluc2t5IC0gaHR0cDovL2ltc2t5LmNvCi0tPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+PCFbQ0RBVEFbI2hvbGRlcl8xODk2OWEwODA4NCB0ZXh0IHsgZmlsbDojQUFBQUFBO2ZvbnQtd2VpZ2h0OmJvbGQ7Zm9udC1mYW1pbHk6QXJpYWwsIEhlbHZldGljYSwgT3BlbiBTYW5zLCBzYW5zLXNlcmlmLCBtb25vc3BhY2U7Zm9udC1zaXplOjEwcHQgfSBdXT48L3N0eWxlPjwvZGVmcz48ZyBpZD0iaG9sZGVyXzE4OTY5YTA4MDg0Ij48cmVjdCB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIGZpbGw9IiNFRUVFRUUiLz48Zz48dGV4dCB4PSIxMy4xNzUyMTg1ODIxNTMzMiIgeT0iMzYuNTU5OTk5OTQyNzc5NTQiPjY0eDY0PC90ZXh0PjwvZz48L2c+PC9zdmc+"
                             data-holder-rendered="true" style="width: 64px; height: 64px;">
                    </a>
                </div>
                <div class="media-body">
                    <h4 class="media-heading">Top aligned media</h4>
                    <p>Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin
                        commodo. Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum
                        nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.</p>
                    <p>Donec sed odio dui. Nullam quis risus eget urna mollis ornare vel eu leo. Cum sociis natoque
                        penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
                </div>
            </div>
            <div class="media">
                <div class="media-left">
                    <a href="#">
                        <img class="media-object" data-src="holder.js/64x64" alt="64x64"
                             src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/PjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+PCEtLQpTb3VyY2UgVVJMOiBob2xkZXIuanMvNjR4NjQKQ3JlYXRlZCB3aXRoIEhvbGRlci5qcyAyLjYuMC4KTGVhcm4gbW9yZSBhdCBodHRwOi8vaG9sZGVyanMuY29tCihjKSAyMDEyLTIwMTUgSXZhbiBNYWxvcGluc2t5IC0gaHR0cDovL2ltc2t5LmNvCi0tPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+PCFbQ0RBVEFbI2hvbGRlcl8xODk2OWEwODA4NCB0ZXh0IHsgZmlsbDojQUFBQUFBO2ZvbnQtd2VpZ2h0OmJvbGQ7Zm9udC1mYW1pbHk6QXJpYWwsIEhlbHZldGljYSwgT3BlbiBTYW5zLCBzYW5zLXNlcmlmLCBtb25vc3BhY2U7Zm9udC1zaXplOjEwcHQgfSBdXT48L3N0eWxlPjwvZGVmcz48ZyBpZD0iaG9sZGVyXzE4OTY5YTA4MDg0Ij48cmVjdCB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIGZpbGw9IiNFRUVFRUUiLz48Zz48dGV4dCB4PSIxMy4xNzUyMTg1ODIxNTMzMiIgeT0iMzYuNTU5OTk5OTQyNzc5NTQiPjY0eDY0PC90ZXh0PjwvZz48L2c+PC9zdmc+"
                             data-holder-rendered="true" style="width: 64px; height: 64px;">
                    </a>
                </div>
                <div class="media-body">
                    <h4 class="media-heading">Top aligned media</h4>
                    <p>Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin
                        commodo. Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum
                        nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.</p>
                    <p>Donec sed odio dui. Nullam quis risus eget urna mollis ornare vel eu leo. Cum sociis natoque
                        penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
                </div>
            </div>
            <div class="media">
                <div class="media-left">
                    <a href="#">
                        <img class="media-object" data-src="holder.js/64x64" alt="64x64"
                             src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9InllcyI/PjxzdmcgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIHZpZXdCb3g9IjAgMCA2NCA2NCIgcHJlc2VydmVBc3BlY3RSYXRpbz0ibm9uZSI+PCEtLQpTb3VyY2UgVVJMOiBob2xkZXIuanMvNjR4NjQKQ3JlYXRlZCB3aXRoIEhvbGRlci5qcyAyLjYuMC4KTGVhcm4gbW9yZSBhdCBodHRwOi8vaG9sZGVyanMuY29tCihjKSAyMDEyLTIwMTUgSXZhbiBNYWxvcGluc2t5IC0gaHR0cDovL2ltc2t5LmNvCi0tPjxkZWZzPjxzdHlsZSB0eXBlPSJ0ZXh0L2NzcyI+PCFbQ0RBVEFbI2hvbGRlcl8xODk2OWEwODA4NCB0ZXh0IHsgZmlsbDojQUFBQUFBO2ZvbnQtd2VpZ2h0OmJvbGQ7Zm9udC1mYW1pbHk6QXJpYWwsIEhlbHZldGljYSwgT3BlbiBTYW5zLCBzYW5zLXNlcmlmLCBtb25vc3BhY2U7Zm9udC1zaXplOjEwcHQgfSBdXT48L3N0eWxlPjwvZGVmcz48ZyBpZD0iaG9sZGVyXzE4OTY5YTA4MDg0Ij48cmVjdCB3aWR0aD0iNjQiIGhlaWdodD0iNjQiIGZpbGw9IiNFRUVFRUUiLz48Zz48dGV4dCB4PSIxMy4xNzUyMTg1ODIxNTMzMiIgeT0iMzYuNTU5OTk5OTQyNzc5NTQiPjY0eDY0PC90ZXh0PjwvZz48L2c+PC9zdmc+"
                             data-holder-rendered="true" style="width: 64px; height: 64px;">
                    </a>
                </div>
                <div class="media-body">
                    <h4 class="media-heading">Top aligned media</h4>
                    <p>Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin
                        commodo. Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum
                        nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.</p>
                    <p>Donec sed odio dui. Nullam quis risus eget urna mollis ornare vel eu leo. Cum sociis natoque
                        penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
                </div>
            </div>
            <!--            分页-->
            <ul class="pagination">
                <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">«</span></a></li>
                <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
                <li><a href="#">2</a></li>
                <li><a href="#">3</a></li>
                <li><a href="#">4</a></li>
                <li><a href="#">5</a></li>
                <li><a href="#" aria-label="Next"><span aria-hidden="true">»</span></a></li>
            </ul>

        </div>
        <div class="col-sm-3">
            <!--            面板-->
            <div class="panel panel-default">
                <div class="panel-heading">最新推荐</div>
                <div class="panel-body">
                    Panel content
                </div>
            </div>
            <div class="panel panel-primary">
                <div class="panel-heading">24小时热门</div>
                <div class="panel-body">
                    Panel content
                </div>
            </div>
        </div>
    </div>
</div>
</body>
</html>
```

效果图：

![image-20230718234521783](imge/WEB开发.assets/image-20230718234521783.png)

###### 案例：登录

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .account{
            /*宽度*/
            width: 400px;
            /*height: 350px;*/
            /*框*/
            border: 1px solid #dddddd;
            /*圆角*/
            border-radius: 10px;
            /*阴影 水平方向 垂直方向 模糊距离 颜色*/
            box-shadow: 5px 5px 5px #aaaaaa;
            
            /*页面布局*/
            /*margin-left: auto;*/
            margin: 200px auto;
            /*内边距*/
            padding: 20px 40px;
        }

        .account h1{
            margin-top: 0;
            margin-bottom: 20px;
            /*字体居中*/
            text-align: center;
        }
    </style>
</head>
<body>
<div class="account">
    <h1>用户登录</h1>
    <form>
        <div class="form-group">
            <label for="exampleInputUsername">用户名或手机号</label>
            <input type="text" name="username" class="form-control" id="exampleInputUsername" placeholder="用户名或手机号">
        </div>
        <div class="form-group">
            <label for="exampleInputPassword">密码</label>
            <input type="password" name="password" class="form-control" id="exampleInputPassword" placeholder="密码">
        </div>
         <button type="submit" class="btn btn-primary">登  录</button>
    </form>
</div>
</body>
</html>
```

效果：

![image-20230719224447911](imge/WEB开发.assets/image-20230719224447911.png)

###### 案例：后台管理

![image-20230720171831820](imge/WEB开发.assets/image-20230720171831820.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .navbar {
            border-radius: 0;
        }
    </style>
</head>
<body>
<div>
    <nav class="navbar navbar-default">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                        data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="#">中国联通XXX系统</a>
            </div>
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav">
                    <li><a href="#">广西</a></li>
                    <li><a href="#">上海</a></li>
                </ul>
                <ul class="nav navbar-nav navbar-right">
                    <li><a href="#">登录</a></li>
                    <li><a href="#">注册</a></li>
                </ul>
            </div>
        </div>
    </nav>

    <div class="container">
        <div>
            <button class="btn btn-primary">新 建</button>
        </div>
        <div style="margin-top: 20px">
            <table class="table table-bordered table-hover table-striped">
                <thead>
                <tr>
                    <th>序号</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>Username</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                    <th scope="row">1</th>
                    <td>Mark</td>
                    <td>Otto</td>
                    <td>@mdo</td>
                </tr>
                <tr>
                    <th scope="row">2</th>
                    <td>Jacob</td>
                    <td>Thornton</td>
                    <td>@fat</td>
                </tr>
                <tr>
                    <th scope="row">3</th>
                    <td>Larry</td>
                    <td>the Bird</td>
                    <td>@twitter</td>
                </tr>
                <tr>
                    <th scope="row">4</th>
                    <td>Larry</td>
                    <td>the Bird</td>
                    <td>@twitter</td>
                </tr>
                </tbody>
            </table>
        </div>
    </div>

</div>
</body>
</html>
```

###### 案例：后台管理+面板

![image-20230720171903591](imge/WEB开发.assets/image-20230720171903591.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <style>
        .navbar {
            border-radius: 0;
        }
    </style>
</head>
<body>
<div>
    <nav class="navbar navbar-default">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                        data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
                    <span class="sr-only">Toggle navigation</span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="#">中国联通XXX系统</a>
            </div>
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav">
                    <li><a href="#">广西</a></li>
                    <li><a href="#">上海</a></li>
                </ul>
                <ul class="nav navbar-nav navbar-right">
                    <li><a href="#">登录</a></li>
                    <li><a href="#">注册</a></li>
                </ul>
            </div>
        </div>
    </nav>


    <div class="container">
        <div class="panel panel-default">
            <div class="panel-heading">表单区域</div>
            <div class="panel-body">
                <form class="form-inline">
                    <div class="form-group">
                        <label class="sr-only" for="exampleInputEmail3">Email address</label>
                        <input type="email" class="form-control" id="exampleInputEmail3" placeholder="Email">
                    </div>
                    <div class="form-group">
                        <label class="sr-only" for="exampleInputPassword3">Password</label>
                        <input type="password" class="form-control" id="exampleInputPassword3" placeholder="Password">
                    </div>

                    <button type="submit" class="btn btn-success">保 存</button>
                </form>
            </div>
        </div>

        <div class="panel panel-default">
            <div class="panel-heading">数据列表</div>
            <!--
            <div class="panel-body">
                <p>...</p>
            </div>
`             -->
            <!-- Table -->
            <table class="table table-bordered table-hover table-striped">
                <thead>
                <tr>
                    <th>序号</th>
                    <th>First Name</th>
                    <th>Last Name</th>
                    <th>操作</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                    <th scope="row">1</th>
                    <td>Mark</td>
                    <td>Otto</td>
                    <td>
                        <a href="" class="btn btn-primary  btn-xs">编辑</a>
                        <a href="" class="btn btn-danger  btn-xs">删除</a>
                    </td>
                </tr>
                <tr>
                    <th scope="row">2</th>
                    <td>Jacob</td>
                    <td>Thornton</td>
                    <td>
                        <a href="" class="btn btn-primary  btn-xs">编辑</a>
                        <a href="" class="btn btn-danger  btn-xs">删除</a>
                    </td>
                </tr>
                <tr>
                    <th scope="row">3</th>
                    <td>Larry</td>
                    <td>the Bird</td>
                    <td>
                        <a href="" class="btn btn-primary  btn-xs">编辑</a>
                        <a href="" class="btn btn-danger  btn-xs">删除</a>
                    </td>
                </tr>
                <tr>
                    <th scope="row">4</th>
                    <td>Larry</td>
                    <td>the Bird</td>
                    <td>
                        <a href="" class="btn btn-primary  btn-xs">编辑</a>
                        <a href="" class="btn btn-danger  btn-xs">删除</a>
                    </td>
                </tr>
                </tbody>
            </table>
        </div>
    </div>

</div>
</body>
</html>
```

###### 图标

图标类型较少，建议使用Font Awesome图标模板

### 1.5  Font Awesome（字体图标）模板

- 下载

  网址：https://fontawesome.dashgame.com/

- 引入

  ```css
  <link rel="stylesheet" href="static/plugins/font-awesome-4.7.0/css/font-awesome.css">
  ```

- 使用

  ```html
  <i class="fa fa-pencil-square" aria-hidden="true"></i>
  ```

  ![image-20230720204306056](imge/WEB开发.assets/image-20230720204306056.png)

### 1.6 JavaScript

-  编程语言，浏览器是解释器
-  DOM和BOM（内置模块）
- 类库（第三方模块）
  - jQuery

```Html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menus {
            width: 200px;
            border: 1px solid red
        }

        .menus .header {
            background-color: gold;
            padding: 20px 10px;
        }
    </style>
</head>
<body>
<div class="menus">
    <!--    绑定JS函数-->
    <div class="header" onclick="myFunc()">大标题</div>
    <div class="item">内容</div>
</div>
<!--JS函数-->
<script type="text/javascript">
    function myFunc() {
        alert("Hello Word")
    }
</script>
</body>
</html>
```

效果图：

![image-20230721154844747](imge/WEB开发.assets/image-20230721154844747.png)

#### 1.6.1 JavaScript代码位置

![image-20230721155703721](imge/WEB开发.assets/image-20230721155703721.png)

##### JS代码存在形式

- 当前HTML中

- JS文件中,导入使用

  ```html
  <script src="static/js/jquery-3.6.0.min.js"></script>
  <script src="static/plugins/bootstrap-3.4.1/js/bootstrap.min.js"></script>
  ```

#### 1.6.2 变量

变量定义：

```javascript
var name = "高倩"
```

变量打印：

```javascript
<script type="text/javascript">
    // 定义变量
    var name = "高倩"
    // 打印变量
    console.log(name);
</script>
```

![image-20230721161802798](imge/WEB开发.assets/image-20230721161802798.png)

#### 1.6.3 数据类型

##### 1、字符串

```javascript
// 声明
var name = "高倩"
var name = String("高倩")

// 常见功能
var v1 = name.length;  // 获取字符串长度
var v2 = name[0];  // 字符串索引，name.charAt(0)
var v3 = name.trim();  // 去除首尾空白
var v3 = name.substring(1, 2)  // 切片，前取后不取
```



###### 案例：跑马灯（字符串）

![20230721-165239](imge/WEB开发.assets/20230721-165239.gif)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>跑马灯</title>
</head>
<body>
<span id="txt">欢迎中国联通领导莅临指导</span>
<script type="text/javascript">
    // 定义函数
    function show() {
        // 1.去HTML中找到某个标签并获取它的位置（DOM）
        var tag = document.getElementById("txt");
        var dataSting = tag.innerText;

        // 2.动态起来，把文本中的第一个字符放在字符串最后面
        var firstChar = dataSting[0]
        var otherString = dataSting.substring(1, dataSting.length)
        var newText = otherString + firstChar

        // 3.在HTML标签中更新内容
        tag.innerText = newText
    }

    // 定时器，定时执行函数1000ms
    setInterval(show, 1000)
</script>
</body>
</html>
```

##### 2、数组

```javascript
// 定义
var v1 = [11,22,33,44];
var v2 = Array([11,22,33,44])
```

```javascript
// 定义
var v1 = [11,22,33,44]
// 获取值
v1[1]
// 修改值
v1[0] = "高倩"
// 尾部追加
v1.push("联通")
// 头部追加
v1.unshift("电信")
// 指定位置追加,第二个参数固定为0
v1.splice(3, 0, "中国")
v1.pop()  // 尾部删除
v1.pop()  // 头部删除
v1.splice(3, 1)  // 索引为3的元素删除

// idx 是数组索引
for (var idx in v1) {
    data = v1[idx]
}
```

```javascript
for (var i= 0; i<v1.length; i++) {
    data = v1[i]
}
```

- for 循环开始时，初始化一个变量 i 为 0,然后在每次循环迭代之前检查条件 i < v1.length,即 i 是否小于数组 v1 的长度。如果条件为真，则执行循环体中的代码。
- 在循环体中，将数组 v1 中索引为 i 的元素赋值给变量 data。这里的 i 是循环变量，用于指定要访问数组元素的位置。

###### 案例：动态数据

![image-20230721174856746](imge/WEB开发.assets/image-20230721174856746.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>跑马灯</title>
</head>
<body>
<ul id="city">
    <!--    <li>北京</li>-->
</ul>
<script type="text/javascript">
    var cityList = ["北京", "上海", "深圳"];
    for (var idx in cityList) {
        var text = cityList[idx]
        // 创建 li标签
        var tag = document.createElement("li");
        // 在li标签添加内容
        tag.innerText = text;
        // 添加到id=city 标签里面，DOM
        var parenTag = document.getElementById("city")
        parenTag.appendChild(tag)
    }
</script>
</body>
</html>
```

##### 3、对象（字典）

```javascript
// 定义字典
info = {
    "name": "高倩",
    "age": 18
}

info1 = {
    name: "高倩",
    age: 20
}
```

```javascript
// 读取字典
console.log(info.age)
console.log(info1["name"])

// 修改字典
info1["name"] = "郭子豪"
console.log(info1)

// 删除字典元素
delete info1["name"]
console.log(info1)

// 字典循环
for (var key in info){
    data = info[key]
    console.log(data)
}
```

###### 案例：动态表格

![image-20230725153802815](imge/WEB开发.assets/image-20230725153802815.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动态数据</title>
</head>
<body>
<table border="1">
    <thead>
    <tr>
        <th>ID</th>
        <th>姓名</th>
        <th>年龄</th>
    </tr>
    </thead>
    <tbody id="body">

    </tbody>
</table>
<script type="text/javascript">
    var dataList = [
        {id: 1, name: "墎值1", age: 19},
        {id: 2, name: "墎值2", age: 19},
        {id: 3, name: "墎值3", age: 19},
        {id: 4, name: "墎值4", age: 19},
        {id: 5, name: "墎值5", age: 19}
    ];
    for (var idx in dataList) {
        var info = dataList[idx];
        var tr = document.createElement("tr");
        for (var key in info) {
            var text = info[key];
            // 创建 td标签
            var td = document.createElement('td');
            // 在td标签添加内容
            td.innerText = text;
            // td标签添加到tr标签里面
            tr.appendChild(td);
        }
        // 添加到id=body 标签里面，DOM
        var bodyTag = document.getElementById("body")
        bodyTag.appendChild(tr)
    }

</script>
</body>
</html>
```

#### 1.6.4 条件语句

```javascript
//=======================
if	(条件) {
    
}
else{
    
}
//=======================
if	(条件) {
    
}
else if (条件) {
    
}
else{
    
}
```

#### 1.6.5 函数

```javascript
// 定义函数
function func(){
    .....
}
// 执行函数
func()
```

#### 1.6.6 jQuery模块

jQuery是一个JavaScript第三方模块(第三方类库)

- 基于jQuery，自己开发一个功能。
- 现成的工具依赖jQuery，例如: BootStrap动态效果。
- 在线参考手册：https://www.jq22.com/chm/jquery/remove.html

##### 下载

最新版本：https://jquery.com

历史版本：https://releases.jquery.com/jquer

##### 应用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

</head>
<body>
<h1 id="txt">中国联通</h1>
<script src="static/jquery-3.6.0.min.js"></script>
<script type="text/javascript">
    // 利用jQuery中的功能实现某些效果
    // 找到id=txt的标签，修改内容
    $("#txt").text("广西移动");
    // document.getElementById("txt").innerText="广西移动"

</script>

</body>
</html>
```

##### 直接寻找标签

- ID选择器`$("#id")`

- 样式选择器`$(".class")`

- 标签选择器`$("h1")`

- 层级选择器`$(".c1 .c2 a")`

  ```html
  <h1 class="c1">中国联通1</h1>
  <hl class="cl>
  	<div class="c2">
  	<span></span>
  	<a></a>
  </div>
  </h1>
  <h1 class="c2">中国联通3</h1>
  ```

- 多选择器`$(".c1, .c2, a")`
- 属性选择器`$("input[name='n1']")`

##### 间接寻找标签

- 找兄弟标签

  ```html
  <div>
      <div>北京</div>
      <div id='c1'>上海</div>
      <div>深圳</div>
      <div>广州</div>
  </div>
  ```

  ```javascript
  $("#c1").prev()  // 上一个
  $("#c1")
  $("#c1").next()  // 下一个
  $("#c1").next().next()  // 下一个下一个
  
  $("#c1").siblings()  // 所有兄弟
  ```

- 找父子

  ```html
  <div>
      <div>
          <div>北京</div>
          <div id='c1'>上海
              <ul>
                  <li>青浦区</li>
                  <li class="p">宝山区</li>
                  <li>浦东新区</li>
              </ul>
          </div>
          <div>深圳</div>
          <div>广州</div>
      </div>
      <div>
          <div>河南1</div>
          <div>河南2</div>
          <div>河南3</div>
          <div>河南4</div>
          <div>河南5</div>
      </div>
  </div>
  ```

  ```javascript
  $('#c1').parent()  // 父亲
  $('#c1').parent().parent()  // 父亲 父亲
    
  $('#c1').childrem()  // 所有儿子
  $('#c1').childrem(".p10")  // 所有儿子中寻找 class = p10
  
  $('#c1').find(".p10"")  // 所有子孙中寻找 class = p10
  $('#c1').find("div"")  // 所有子孙中寻找 div
  ```

##### 案例：菜单切换

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .menus {
            width: 200px;
            height: 800px;
            /*边框*/
            border: 2px dashed red;
        }

        .menus .header {
            background-color: gold;
            padding: 10px 5px;
            border-bottom: 3px dotted black;
            
            /*鼠标变小手*/
            cursor: pointer;
        }

        /*鼠标放到上面显示样式设置*/
        .header:hover {
            color: orchid;
            font-size: 50px;
        }

        .menus .content a {
            display: block;
            padding: 5px 5px;
            /*下划线 点线 灰色*/
            border-bottom: 3px dotted green;
        }

        .hide {
            /*隐藏*/
            display: none;
        }


    </style>
</head>
<body>
<div>
    <div class="menus">
        <div class="item">
            <!-- clickMe(this)  this代表本标签 -->
            <div class="header" onclick="clickMe(this)">上海</div>
            <div class="content">
                <a href="">宝山区</a>
                <a href="">青浦区</a>
                <a href="">浦东区</a>
                <a href="">普陀区</a>
            </div>
        </div>

        <div class="item">
            <div class="header" onclick="clickMe(this)">北京</div>
            <div class="content hide">
                <a href="">海淀区</a>
                <a href="">朝阳区</a>
                <a href="">大兴区</a>
                <a href="">昌平区</a>
            </div>
        </div>

        <div class="item">
            <div class="header" onclick="clickMe(this)">北京1</div>
            <div class="content hide">
                <a href="">海淀区</a>
                <a href="">朝阳区</a>
                <a href="">大兴区</a>
                <a href="">昌平区</a>
            </div>
        </div>

        <div class="item">
            <div class="header" onclick="clickMe(this)">北京2</div>
            <div class="content hide">
                <a href="">海淀区</a>
                <a href="">朝阳区</a>
                <a href="">大兴区</a>
                <a href="">昌平区</a>
            </div>
        </div>
    </div>
</div>

<script src="static/jquery-3.6.0.min.js"></script>
<script>
    function clickMe(self) {
        // $(self) 表示当前的标签
        $(self).next().removeClass("hide")
        $(self).parent().siblings().find(".content").addClass("hide")
    }
</script>
</body>
</html>
```

效果

![20230818-174557](imge/WEB开发.assets/20230818-174557.gif)



##### 操作样式

- addClass 添加属性

  ```javascript
      // th标签添加属性
      $("th").addClass("text-center")
  ```

  

- removeClass 移除属性

- hasClass  检查属性是否存在

##### 操作值

```html
<div id='c1'>内容</div>
```

```javascript
$("#c1").text()  // 获取文本内容
$("#c1").text("休息")  // 设置文本内容
```



```html
<input type='text' id='c2' />
```

```javascript
$('#c2').val()  // 获取用户输入值
$('#c2').val("哈哈哈")  // 设置值
```

##### 案例：动态创建数据

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<input type="text" id="txtUser" placeholder="用户名">
<input type="text" id="txtEmail" placeholder="邮箱">
<input type="button" value="提交" onclick="getInfo()"/>

<ul id="view">
</ul>
<script src="static/jquery-3.6.0.min.js"></script>
<script>
    function getInfo() {
        // 1.获取用户输入的用户名和密码
        var username = $("#txtUser").val();
        var email = $("#txtEmail").val();
        // 拼接内容
        var dataString = username + "-" +email

        // 2.创建li标签,在li标签内部写入内容 $("<li>")
        var newLI = $("<li>").text(dataString);

        // 3.把新创建的li标签添加到ul里面
        $("#view").append(newLI)
    }
</script>
</body>
</html>
```

##### 事件绑定

DOM方法

```html
<input type="button" value="提交" onclick="getInfo()"/>

<script>
    function getInfo() {

    }
</script>
```

jQuery方法

```html
<ul>
    <li>百度</li>
    <li>谷歌</li>
    <li>搜狗</li>
</ul>
<script>
    $("li").click(function(){
        // 点击li标签时，自动执行这个函数；
        // $(this) 当前点击的是那个标签。
    });
</script>
```

##### 案例：绑定事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul id="log">
    <li>百度</li>
    <li>谷歌</li>
    <li>搜狗</li>
</ul>

<ul id="remove">
    <li>百度</li>
    <li>谷歌</li>
    <li>搜狗</li>
</ul>
<script src="static/jquery-3.6.0.min.js"></script>
<script>
    // ==============点击标签打印标签===============
    $('#log').find("li").click(function () {
        // 点击li标签时，自动执行这个函数；
        // $(this) 当前点击的是那个标签。
        var text = $(this).text();
        // 打印
        console.log(text);
        // 弹窗
        alert(text)
    });

    // ==============点击标签删除标签===============
    $("#remove").find("li").click(function () {
        // 点击li标签时，自动执行这个函数；
        // $(this) 当前点击的是那个标签。
        $(this).remove();
    });
</script>
</body>
</html>
```

##### 删除标签

```html
<div id='c1'>内容</div>

$("#c1").remove();
```

案例：参见：绑定事件



> 当页面框架加载完成之后执行代码：
>
> ```
> $(function () {
>     // ==============点击标签删除标签===============
>     $("#remove").find("li").click(function () {
>     // 点击li标签时，自动执行这个函数；
>     // $(this) 当前点击的是那个标签。
>     $(this).remove();
> });
> ```

##### 案例：表格删除行

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        th, td {
            border: 1px solid;

        }
        input{
            width: 50px;
            color: #ff2424;
            background-color:gold;
            /*border: 10px  red;*/
        }
    </style>
</head>
<body>
<table>
    <thead>
    <tr>
        <th>ID</th>
        <th style="width: 50px;">name</th>
        <th style="width: 50px;">操作</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>1</td>
        <td>吴佩其</td>
        <td>
            <input type="button" value="删除"/>
        </td>
    </tr>
    <tr>
        <td>2</td>
        <td>吴佩其</td>
        <td>
            <input type="button" value="删除"/>
        </td>

    </tr>
    <tr>
        <td>3</td>
        <td>吴佩其</td>
        <td>
            <input type="button" value="删除"/>
        </td>
    </tr>
    </tbody>
</table>
<script src="static/jquery-3.6.0.min.js"></script>
<script>
    // input 标签添加delete属性
    $("input").addClass("delete")
    // 删除表格行
    $(function () {
        $(".delete").click(function () {
            $(this).parent().parent().remove();
        })
    })
</script>

</body>
</html>
```

效果：

![20230920-183417](imge/WEB开发.assets/20230920-183417.gif)











### 1.7 DOM

DOM模块可以对HTML页面种的标签进行操作。

```javascript
// 根据ID获取标签
var tag = docunebt.getElementById("xxx");

// 获取标签中的文本
tag.innerText

// 修改标签中的文本
tag.innerText = "哈哈哈哈";
```

```javascript
// 创建div标签 <div>哈哈哈哈</div>
var tag = document.createElement('div');
// 在div标签添加内容
td.innerText = "哈哈哈哈";
```

案例：参见1.6

#### 1.7. 1 事件绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件</title>
</head>
<body>
<input type="text" placeholder="请输入内容" id="txtUser">
<!--onclick 单击  ondblclick 双击-->
<input type="button" value="点击添加" onclick="addCityInfo()">
<ul id="city">

</ul>
<script type="text/javascript">
    function addCityInfo() {
        // 添加到id="city"标签
        var parentTag = document.getElementById("city")

        // 1.找到输入标签
        var txtTag = document.getElementById("txtUser");
        // 2.先获取输入框中用户输入的内容
        var newString = txtTag.value;

        // 判断用户输入是否为空
        if (newString.trimStart().length > 0) {
            // 3.创建li标签
            var newTag = document.createElement('li');
            newTag.innerText = newString;


            // 4.将输入框内容清空
            txtTag.value = "";

            // 添加li标签
            parentTag.appendChild(newTag);
        } else {
            // 将输入框内容清空
            txtTag.value = "";
            alert("输入不能为空")
        }

    }

</script>
</body>
</html>
```

DOM可以实现很多功能，但是比较繁琐，页面上的效果可以用jQuerry等类库或框架来实现

### 1.8 前端总结

- HTML

- CSS

- JavaScript、jQuery

- BootStrap(动态效果依赖 jQuery)


bootstrap-datepicker(日期选择器插件)下载源：

- github：https://github.com/uxsolutions/bootstrap-datepicker
- gitee:https://gitee.com/mirrors/bootstrap-datepicker/tree/master

#### 案例：添加数据页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--引入css插件-->
    <link rel="stylesheet" href="static/plugins/bootstrap-3.4.1/css/bootstrap.css">
    <link rel="stylesheet" href="static/plugins/font-awesome-4.7.0/css/font-awesome.css">
    <link rel="stylesheet" href="static/plugins/bootstrap-datepicker/css/bootstrap-datepicker.css">
</head>
<body>
<div class="container">
    <form class="form-horizontal" style="margin-top: 20px;">
        <div class="row clearfix">
            <div class="col-xs-6">
                <div class="form-group">
                    <label class="col-sm-2 control-label">姓名</label>
                    <div class="col-sm-10">
                        <input type="text" class="form-control" placeholder="姓名">
                    </div>
                </div>
            </div>
            <div class="col-xs-6">
                <div class="form-group">
                    <label class="col-sm-2 control-label">年龄</label>
                    <div class="col-sm-10">
                        <input type="number" class="form-control" placeholder="年龄">
                    </div>
                </div>
            </div>
        </div>
        <div class="row clearfix">
            <div class="col-xs-6">
                <div class="form-group">
                    <label class="col-sm-2 control-label">薪资</label>
                    <div class="col-sm-10">
                        <input type="number" class="form-control" placeholder="薪资">
                    </div>
                </div>
            </div>
            <div class="col-xs-6">
                <div class="form-group">
                    <label class="col-sm-2 control-label">部门</label>
                    <div class="col-sm-10">
                        <select class="form-control">
                            <option>IT部门</option>
                            <option>销售部门</option>
                            <option>运营部</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
        <div class="row clearfix">
            <div class="col-xs-6">
                <div class="form-group">
                    <label class="col-sm-2 control-label">入职日期</label>
                    <div class="col-sm-10">
                        <input type="date" class="form-control" placeholder="入职日期">
                    </div>
                </div>
            </div>
            <div class="col-xs-6">
                <div class="form-group">
                    <label class="col-sm-2 control-label">离职日期</label>
                    <div class="col-sm-10">
                        <input type="text" id="dt" class="form-control" placeholder="离职日期">

                    </div>
                </div>
            </div>
        </div>
        <div class="row clearfix">
            <div class="col-xs-6">
                <div class="form-group">
                    <div class="col-sm-offset-2 col-sm-10">
                        <button type="submit" class="btn btn-primary">提 交</button>
                    </div>
                </div>
            </div>
        </div>
    </form>
</div>
<!--引入js模板-->
<script src="static/js/jquery-3.6.0.min.js"></script>
<script src="static/plugins/bootstrap-3.4.1/js/bootstrap.js"></script>
<script src="static/plugins/bootstrap-datepicker/js/bootstrap-datepicker.js"></script>
<!--引入汉化包-->
<script src="static/plugins/bootstrap-datepicker/locales/bootstrap-datepicker.zh-CN.min.js"></script>
<script>
    $(function () {
        $('#dt').datepicker({
            format: "yyyy-mm-dd",
            startDate: "1900-01-01",
            // 中文显示
            language: "zh-CN",
            autoclose: true,
            // 清楚按钮
            clearBtn: true
        })

    })
</script>
</body>
</html>
```

效果：

![20230926-171722](imge/WEB开发.assets/20230926-171722.gif)

# 二、Mysql

Mysql安装及使用，参见：mysql教程

python操作Mysql，参见：python教程

## 案例：

mysql数据库表结构：

- admin

  ```mysql
  CREATE TABLE `website`.`website`  (
    `id` int NOT NULL AUTO_INCREMENT,
    `username` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
    `password` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
    `mobile` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL,
    PRIMARY KEY (`id`) USING BTREE
  ) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci ROW_FORMAT = Dynamic;
  ```



### 1 新增用户

app.py

```python
"""
程序说明：
    功能：网站入口
模块说明：
    1、flask:WEB框架模块
        模块安装 pip install Flask
"""

# here put the import lib
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/add/user", methods=["GET", "POST"])
def add_user():
    if request.method == "GET":
        return render_template("add_user.html")
	
    username = request.form.get("user")
    password = request.form.get("pwd")
    mobile = request.form.get("mobile")

    # 1.连接mysql
    import pymysql
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="website", password="website123", charset='utf8',
                           db='website')
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)
    # 2.执行SQL
    sql = "insert into admin(username,password,mobile) values (%s,%s,%s)"
    cursor.execute(sql, [username, password, mobile])
    conn.commit()
    # 3.关闭连接
    cursor.close()
    conn.close()
    return "添加成功"
if __name__ == '__main__':
    app.run(debug=True)

```

add_user.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1>添加用户</h1>
<form method="post" action="/add/user">
    <input type="text" name="user" placeholder="用户名">
    <input type="text" name="pwd" placeholder="密码">
    <input type="text" name="mobile" placeholder="手机号">
    <input type="submit" value="提 交">
</form>
</body>
</html>
```

效果：

![20230927-175442](imge/WEB开发.assets/20230927-175442.gif)



### 2 查询所有用户





# 三、Django

# 四、Flask

学习网站

1. https://www.yiibai.com/flask/flask_overview.html

## 1 简介

### 什么是Web框架？

Web应用程序框架或简单的Web框架表示一组库和模块，它们使Web应用程序开发人员能够编写应用程序，而不必担心如协议，线程管理等低层细节。

### 什么是Flask？

Flask是一个用Python编写的Web应用程序框架。 它由Armin Ronacher开发，他领导着一个名为Pocco的Python爱好者的国际组织。 Flask基于Werkzeug WSGI工具包和Jinja2模板引擎。 这两个都是Pocco项目。

### WSGI

Web服务器网关接口(WSGI)已被采纳为Python Web应用程序开发的标准。 WSGI是Web服务器和Web应用程序之间通用接口的规范。

### WERKZEUG

它是一个WSGI工具包，它实现了请求，响应对象和其他实用程序功能。 这可以在其上构建Web框架。 Flask框架使用Werkzeug作为其一个基础模块之一。

### Jinja2

jinja2是Python的流行模板引擎。 网页模板系统将模板与特定的数据源结合起来呈现动态网页。

Flask通常被称为**微框架**。 它旨在保持应用程序的核心简单且可扩展。 Flask没有用于数据库处理的内置抽象层，也没有形成验证支持。 相反，Flask支持扩展以将这些功能添加到应用程序中。

## 2 安装

pip install Flask

## 3 应用程序

要测试Flask安装是否成功，在编辑器中输入以下代码，并保存到文件:`Hello.py` 中。

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World'

if __name__ == '__main__':
    app.run()
```

在项目中导入`Flask`模块是强制性的。 Flask类的一个对象是WSGI应用程序。

Flask构造函数将当前模块的名称(`__name__`)作为参数。

Flask类的`route()`函数是一个装饰器，它告诉应用程序哪个URL应该调用相关的函数。

```python
app.route(rule, options)
```

- *rule* 参数表示与该函数绑定的URL。
- *options* 是要转发给底层Rule对象的参数列表。

在上面的例子中，`'/'` URL与`hello_world()`方法绑定。 因此，在浏览器中打开Web服务器的主页时，将呈现此函数的输出。

最后，Flask类的`run()`方法在本地开发服务器上运行应用程序。

```python
app.run(host, port, debug, options)
```

上面方法中的所有参数都是可选的，作用如下表描述说明 

| 编号 |  参数   | 描述                                                         |
| :--: | :-----: | :----------------------------------------------------------- |
|  1   |  host   | 监听的主机名。默认为`127.0.0.1`(localhost)。 设置为`'0.0.0.0'`使服务器在外部可用 |
|  2   |  port   | 监听端口号，默认为:`5000`                                    |
|  3   |  debug  | 默认为:`false`。 如果设置为:`true`，则提供调试信息           |
|  4   | options | 被转发到底层的Werkzeug服务器。                               |

### 调试模式

Flask应用程序通过调用`run()`方法来启动。 但是，当应用程序正在开发中时，应该为代码中的每个更改手动重新启动它。 为了避免这种不便，可以启用调试支持。 如果代码改变，服务器将自动重新加载。 它还将提供一个有用的调试器来跟踪应用程序中的错误(如果有的话)。

在运行或将调试参数传递给`run()`方法之前，通过将应用程序对象的调试属性设置为`True`来启用调试模式。

```python
app.debug = True
app.run()
app.run(debug = True)
```

## 4 路由

现代Web框架使用路由技术来帮助用户记住应用程序URL。 无需从主页导航即可直接访问所需页面。

Flask中的`route()`装饰器用于将URL绑定到函数。 例如 -

```python
@app.route('/hello')
def hello_world():
    return 'hello world'
```

这里，URL `/hello`规则绑定到`hello_world()`函数。 因此，如果用户访问URL : `http://localhost:5000/hello` ，就会调用`hello_world()`函数，这个函数中的执行的结果输出将在浏览器中呈现。

应用程序对象的`add_url_rule()`函数也可用于将URL与函数绑定，如上例所示，使用`route()`。

```python
def hello_world():
    return 'hello world'

app.add_url_rule('/', 'hello', hello_world)
```

## 5 变量规则

可以通过将可变部分添加到规则参数来动态构建URL。 这个变量部分被标记为`<variable-name>`。 它作为关键字参数传递给规则所关联的函数。

在以下示例中，`route()`装饰器的规则参数包含附加到URL `/hello`的`<name>`变量部分。 因此，如果在浏览器中输入URL: `http://localhost:5000/hello/YiibaiYiibai`，那么 ‘YiibaiYiibai’ 将作为参数提供给`hello()`函数。

参考如下代码 - 

```python
from flask import Flask
app = Flask(__name__)

@app.route('/hello/<name>')
def hello_name(name):
    return 'Hello %s!' % name

if __name__ == '__main__':
    app.run(debug = True)
```

将上面的脚本保存到文件:`hello.py`，并从Python shell运行它。
![img](imge/WEB开发.assets/895060558_49557.png)

接下来，打开浏览器并输入URL => `http://localhost:5000/hello/YiibaiYiibai`。在浏览器中输出如下所示 -
![img](imge/WEB开发.assets/539060559_62745.png)

除了默认的字符串变量部分之外，还可以使用以下转换器构造规则 -

| 编号 | 转换器 | 描述                            |
| ---- | ------ | ------------------------------- |
| 1    | int    | 接受整数                        |
| 2    | float  | 对于浮点值                      |
| 3    | path   | 接受用作目录分隔符的斜杠符(`/`) |

在下面的代码中，使用了所有这些构造函数。

```python
from flask import Flask
app = Flask(__name__)

@app.route('/blog/<int:postID>')
def show_blog(postID):
    return 'Blog Number %d' % postID

@app.route('/rev/<float:revNo>')
def revision(revNo):
    return 'Revision Number %f' % revNo

if __name__ == '__main__':
    app.run()
```

从Python Shell运行上述代码。 在浏览器中访问URL => `http:// localhost:5000/blog/11`。

给定的数字值作为:`show_blog()`函数的参数。 浏览器显示以下输出 -

```shell
Blog Number 11
```

在浏览器中输入此URL - `http://localhost:5000/rev/1.1`

`revision()`函数将浮点数作为参数。 以下结果出现在浏览器窗口中 -

```shell
Revision Number 1.100000
```

Flask的URL规则基于Werkzeug的路由模块。 这确保了形成的URL是唯一的，并且基于Apache制定的先例。

考虑以下脚本中定义的规则 -

```python
from flask import Flask
app = Flask(__name__)

@app.route('/flask')
def hello_flask():
    return 'Hello Flask'

@app.route('/python/')
def hello_python():
    return 'Hello Python'

if __name__ == '__main__':
    app.run()
```

两条规则看起来都很相似，但在第二条规则中，使用了尾部斜线(`/`)。 因此，它变成了一个规范的URL。 因此，使用`/python`或`/python/`返回相同的输出。 但是，在第一条规则的情况下， URL:`/flask/`会导致`404 Not Found`页面。

## 6 URL构建

`url_for()`函数对于动态构建特定函数的URL非常有用。 该函数接受函数的名称作为第一个参数，并接受一个或多个关键字参数，每个参数对应于URL的变量部分。

以下脚本演示了使用`url_for()`函数。

```python
from flask import Flask, redirect, url_for
app = Flask(__name__)

@app.route('/admin')
def hello_admin():
    return 'Hello Admin'

@app.route('/guest/<guest>')
def hello_guest(guest):
    return 'Hello %s as Guest' % guest

@app.route('/user/<name>')
def user(name):
    if name =='admin':
        return redirect(url_for('hello_admin'))
    else:
        return redirect(url_for('hello_guest',guest = name))

if __name__ == '__main__':
    app.run(debug = True)
```

上面的脚本有一个函数用户(名称)，它接受来自URL的参数值。

`User()`函数检查收到的参数是否与’admin’匹配。 如果匹配，则使用`url_for()`将应用程序重定向到`hello_admin()`函数，否则将该接收的参数作为`guest`参数传递给`hello_guest()`函数。

保存上面的代码到一个文件:*hello.py*，并从Python shell运行。

打开浏览器并输入URL - `http://localhost:5000/user/admin`

浏览器中的应用程序响应输出结果是 -

```shell
Hello Admin
```

在浏览器中输入以下URL - `http://localhost:5000/user/mvl`

应用程序响应结果现在变为 -

```shell
Hello mvl as Guest
```

## 7 HTTP方法

Http协议是万维网数据通信的基础。 它协议定义了从指定URL中检索不同数据的方法。

下表概括了不同的http方法 -

| 编号 | 方法   | 描述                                                         |
| ---- | ------ | ------------------------------------------------------------ |
| 1    | GET    | 将数据以未加密的形式发送到服务器，这最常用的方法。           |
| 2    | HEAD   | 与GET相同，但没有响应主体                                    |
| 3    | POST   | 用于将HTML表单数据发送到服务器。通过POST方法接收的数据不会被服务器缓存。 |
| 4    | PUT    | 用上传的内容替换目标资源的所有当前表示。                     |
| 5    | DELETE | 删除由URL给出的所有目标资源的所有表示                        |

默认情况下，Flask路由响应GET请求。 但是，可以通过为`route()`装饰器提供方法参数来更改此首选项。

为了演示在URL路由中使用POST方法，首先创建一个HTML表单并使用POST方法将表单数据发送到URL。

将以下脚本保存到文件:`login.html`

```html
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title>Flask HTTP请求方法处理</title>
</head>
   <body>
      <form action = "http://localhost:5000/login" method = "post">
         <p>输入姓名:</p>
         <p><input type = "text" name = "name" value=""/></p>
         <p><input type = "submit" value = "提交" /></p>
      </form>

   </body>
</html>
```

现在在Python shell中输入以下脚本。

```python
from flask import Flask, redirect, url_for, request
app = Flask(__name__)

@app.route('/success/<name>')
def success(name):
    return 'welcome %s' % name

@app.route('/login',methods = ['POST', 'GET'])
def login():
    if request.method == 'POST':
        user = request.form['name']
        return redirect(url_for('success',name = user))
    else:
        user = request.args.get('name')
        return redirect(url_for('success',name = user))

if __name__ == '__main__':
    app.run(debug = True)
```

开发服务器开始运行后，在浏览器中打开`login.html`，在文本字段中输入名称(如:*maxsu* )并单击**提交**。
![img](imge/WEB开发.assets/468090526_96803.png)

表单数据被提交到`<form>`标签的`action`属性指定的URL。

`http://localhost:5000/login`被映射到`login()`函数。 由于服务器已通过POST方法接收数据，因此从表单数据获得`'name'`参数的值，通过以下方式-

```python
user = request.form['name']
```

它作为可变部分传递给URL:`/success`。 浏览器在窗口中显示欢迎消息。
![img](imge/WEB开发.assets/948090526_96515.png)

将`login.html`中的方法参数更改为`GET`并在浏览器中再次打开。 在服务器上收到的数据是通过GET方法。 `'name'`参数的值现在通过以下方式获得 -

```python
User = request.args.get('name')
```

这里，`args`是字典对象，它包含一系列表单参数及其对应值。 与之前一样，与`'name'`参数对应的值将传递到URL:`/success`。

## 8 模板

```python
app = Flask(__name__)

@app.route('/')
def index():
   return render_template(‘hello.html’)

if __name__ == '__main__':
   app.run(debug = True)
```

Flask将尝试在该脚本所在的同一文件夹中查找`templates`文件夹中的HTML文件。使用模板的应用程序目录结构如下所示 - 

```shell
app.py
hello.py
    templates
        hello.html
        register.html
        ....
```

术语“Web模板系统”是指设计一个HTML脚本，其中可以动态插入变量数据。 Web模板系统由模板引擎，某种数据源和模板处理器组成。

Flask使用jinga2模板引擎。 Web模板包含用于变量和表达式(这些情况下为Python表达式)的HTML语法散布占位符，这些变量和表达式在模板呈现时被替换为值。

以下代码在模板(*templates*)文件夹中保存为:*hello.html*。

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask HTTP请求方法处理</title>
</head>
   <body>

      <h1>Hello {{ name }}!</h1>

   </body>
</html>
```

接下来，将以下代码保存在*app.py*文件中，并从Python shell运行 - 

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/hello/<user>')
def hello_name(user):
    return render_template('hello.html', name = user)

if __name__ == '__main__':
    app.run(debug = True)
```

在开发服务器开始运行时，打开浏览器并输入URL为 - `http://localhost:5000/hello/maxsu`

URL的可变部分插入`{{name}}`占位符处。

![img](imge/WEB开发.assets/821090558_69765.png)

Jinja2模板引擎使用以下分隔符来从HTML转义。

- `{% ... %}` 用于多行语句
- `{{ ... }}` 用于将表达式打印输出到模板
- `{# ... #}` 用于未包含在模板输出中的注释
- `# ... ##` 用于单行语句

在以下示例中，演示了在模板中使用条件语句。 `hello()`函数的URL规则接受整数参数。 它传递给`hello.html`模板。 在它里面，收到的数字(标记)的值被比较(大于或小于50)，因此在HTML执行了有条件渲染输出。

Python脚本如下 -

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/hello/<int:score>')
def hello_name(score):
    return render_template('hello.html', marks = score)

if __name__ == '__main__':
    app.run(debug = True)
```

模板文件:*hello.html* 的HTML模板脚本如下 -

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask模板示例</title>
</head>
   <body>

      {% if marks>50 %}
      <h1> 通过考试！</h1>
      {% else %}
      <h1>未通过考试！</h1>
      {% endif %}

   </body>
</html>
```

请注意，条件语句`if-else`和`endif`包含在分隔符`{%..。%}`中。

运行Python脚本并访问URL=> `http://localhost/hello/60` ，然后访问 `http://localhost/hello/59`，以有条件地查看HTML输出。

Python循环结构也可以在模板内部使用。 在以下脚本中，当在浏览器中打开URL => `http:// localhost:5000/result`时，`result()`函数将字典对象发送到模板文件:*results.html* 。

*result.html* 的模板部分采用for循环将字典对象`result{}`的键和值对呈现为HTML表格的单元格。

从Python shell运行以下代码。

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/result')
def result():
    dict = {'phy':59,'che':60,'maths':90}
    return render_template('result.html', result = dict)

if __name__ == '__main__':
    app.run(debug = True)
```

将以下HTML脚本保存为模板文件夹(*templates*)中的模板文件:*result.html* 。

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask模板示例</title>
</head>
   <body>
      <table border = 1>
         {% for key, value in result.items() %}
            <tr>
               <th> {{ key }} </th>
               <td> {{ value }} </td>
            </tr>
         {% endfor %}
      </table>
   </body>
</html>
```

在这里，与For循环相对应的Python语句包含在`{%...%}`中，而表达式键和值放在`{{}}`中。

开发开始运行后，在浏览器中打开`http://localhost:5000/result`以获得以下输出。

![img](imge/WEB开发.assets/671130559_61033.png)

## 9 静态文件

Web应用程序通常需要一个静态文件，例如支持显示网页的JavaScript文件或CSS文件。 通常，可以通过配置Web服务器提供这些服务，但在开发过程中，这些文件将从包中的静态文件夹或模块旁边提供，它将在应用程序的`/static`上提供。

使用特殊的端点“静态”来为静态文件生成URL。

在以下示例中，`index.html`中的HTML按钮的`OnClick`事件调用`hello.js`中定义的javascript函数，该函数在Flask应用程序的URL => `/` 中呈现。

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

if __name__ == '__main__':
    app.run(debug = True)
```

*index.html* 中的HTML脚本如下所示。

```html
<html>
   <head>
      <script type = "text/javascript" 
         src = "{{ url_for('static', filename = 'hello.js') }}" ></script>
   </head>
   <body>
      <input type = "button" onclick = "sayHello()" value = "Say Hello" />
   </body>
</html>
```

文件:*hello.js* 中定义包含 `sayHello()` 函数。

```js
function sayHello() {
   alert("Hello World")
}
```

## 10 请求对象

来自客户端网页的数据作为全局请求对象发送到服务器。要处理请求数据，请求对旬应该从Flask模块导入。

请求对象的重要属性如下所列 ：

- `form` - 它是包含表单参数及其值的键和值对的字典对象。
- `args` - 解析问号(`?`)后的URL部分查询字符串的内容。
- `cookies` - 保存Cookie名称和值的字典对象。
- `file` - 与上传文件有关的数据。
- `method` - 当前请求方法。

## 11 表单处理

我们已经看到，可以在URL规则中指定http方法。URL映射的函数接收到的表单数据可以以字典对象的形式收集，并将其转发给模板以在相应的网页上呈现它。

在以下示例中，URL => `/` 呈现具有表单的网页(*student.html*)。填充的数据会提交到触发`result()`函数的URL => `/result` 中。

`results()`函数收集字典对象中`request.form`中存在的表单数据，并将其发送给*result.html* 并显示出来。

该模板动态呈现表单数据的HTML表格。

下面给出的是Python的应用程序代码 -

```python
from flask import Flask, render_template, request
app = Flask(__name__)

@app.route('/')
def student():
    return render_template('student.html')

@app.route('/result',methods = ['POST', 'GET'])
def result():
    if request.method == 'POST':
        result = request.form
        return render_template("result.html",result = result)

if __name__ == '__main__':
    app.run(debug = True)
```

以下是 *student.html* 的HTML脚本的代码。

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask示例</title>
</head>
   <body>

      <form action = "http://localhost:5000/result" method = "POST">
         <p>姓名 <input type = "text" name = "Name" /></p>
         <p>物理分数: <input type = "text" name = "Physics" /></p>
         <p>化学分数: <input type = "text" name = "Chemistry" /></p>
         <p>数学分数: <input type ="text" name = "Mathematics" /></p>
         <p><input type = "submit" value = "提交" /></p>
      </form>
   </body>
</html>
```

模板代码(result.html)在下面给出 -

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask示例</title>
</head>
   <body>

      <table border = 1>
         {% for key, value in result.items() %}
            <tr>
               <th> {{ key }} </th>
               <td> {{ value }} </td>
            </tr>
         {% endfor %}
      </table>

   </body>
</html>
```

运行Python脚本，并在浏览器中输入URL => `http://localhost:5000/` 。结果如下所示 - 

![img](imge/WEB开发.assets/302150517_77617.png)

当点击**提交**按钮时，表单数据以HTML表格的形式呈现在*result.html* 中，如下所示 - 

![img](imge/WEB开发.assets/441150518_88138.png)

## 12 Cookies处理

Cookie以文本文件的形式存储在客户端计算机上。 其目的是记住和跟踪与客户使用有关的数据，以获得更好的访问体验和网站统计。

Request对象包含一个`cookie`的属性。 它是所有cookie变量及其对应值的字典对象，客户端已发送。 除此之外，cookie还会存储其到期时间，路径和站点的域名。

在Flask中，cookies设置在响应对象上。 使用`make_response()`函数从视图函数的返回值中获取响应对象。 之后，使用响应对象的`set_cookie()`函数来存储cookie。

重读cookie很容易。 可以使用`request.cookies`属性的`get()`方法来读取cookie。

在下面的Flask应用程序中，当访问URL => `/` 时，会打开一个简单的表单。

```python
@app.route('/')
def index():
    return render_template('index.html')
```

这个HTML页面包含一个文本输入，完整代码如下所示 - 

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask Cookies示例</title>
</head>
   <body>

      <form action = "/setcookie" method = "POST">
         <p><h3>Enter userID</h3></p>
         <p><input type = 'text' name = 'name'/></p>
         <p><input type = 'submit' value = '登录'/></p>
      </form>

   </body>
</html>
```

表单提交到URL => `/setcookie`。 关联的视图函数设置一个Cookie名称为:`userID`，并的另一个页面中呈现。

```python
@app.route('/setcookie', methods = ['POST', 'GET'])
def setcookie():
   if request.method == 'POST':
        user = request.form['name']

        resp = make_response(render_template('readcookie.html'))
        resp.set_cookie('userID', user)

        return resp
```

`readcookie.html` 包含超链接到另一个函数`getcookie()`的视图，该函数读回并在浏览器中显示cookie值。

```python
@app.route('/getcookie')
def getcookie():
    name = request.cookies.get('userID')
    return '<h1>welcome '+name+'</h1>'
```

完整的应用程序代码如下 - 

```python
from flask import Flask
from flask import render_template
from flask import request
from flask import make_response


app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/setcookie', methods = ['POST', 'GET'])
def setcookie():
    if request.method == 'POST':
        user = request.form['name']

        resp = make_response(render_template('readcookie.html'))
        resp.set_cookie('userID', user)
        return resp

@app.route('/getcookie')
def getcookie():
    name = request.cookies.get('userID')
    print (name)
    return '<h1>welcome, '+name+'</h1>'

if __name__ == '__main__':
    app.run(debug = True)
```

运行该应用程序并访问URL => `http://localhost:5000/`
![img](imge/WEB开发.assets/564150525_69844.png)
设置cookie的结果如下所示 -
![img](imge/WEB开发.assets/460150524_58559.png)

重读cookie的输出如下所示 -
![img](imge/WEB开发.assets/399150525_35352.png)

## 13 Sessions会话

与Cookie不同，会话数据存储在服务器上。 会话是客户端登录到服务器并注销的时间间隔。 需要在此会话中进行的数据存储在服务器上的临时目录中。

与每个客户端的会话分配一个会话ID。 会话数据存储在cookie顶部，服务器以加密方式签名。 对于这种加密，Flask应用程序需要一个定义`SECRET_KEY`。

会话对象也是一个包含会话变量和关联值的键值对的字典对象。

例如，要设置`'username'`会话变量，请使用语句 -

```python
Session['username'] = 'admin'
```

要删除会话变量，请使用`pop()`方法。

```python
session.pop('username', None)
```

以下代码是Flask中会话如何工作的简单演示。 URL => `'/'` 提示用户登录，因为会话变量`username`没有设置。

```python
@app.route('/')
def index():
   if 'username' in session:
      username = session['username']
         return 'Logged in as ' + username + '<br>' + \
         "<b><a href = '/logout'>click here to log out</a></b>"
   return "You are not logged in <br><a href = '/login'></b>" + \
      "click here to log in</b></a>"
```

当用户浏览到URL=>`'/login'`时，`login()`函数显示视图，因为它是通过GET方法调用的，所以打开一个登录表单。

表单填写后重新提交到URL=> `/login`，现在会话变量被设置。 应用程序被重定向到URL=> `/`。 这时找到会话变量:`username`。

```python
@app.route('/login', methods = ['GET', 'POST'])
def login():
   if request.method == 'POST':
      session['username'] = request.form['username']
      return redirect(url_for('index'))
   return '''
   <form action = "" method = "post">
      <p><input type = text name = "username"/></p>
      <p<<input type = submit value = Login/></p>
   </form>
   '''
```

该应用程序还包含一个`logout()`视图函数，它删除’username’会话变量的值。 再次 URL 跳转到 ‘/‘ 显示开始页面。

```python
@app.route('/logout')
def logout():
   # remove the username from the session if it is there
   session.pop('username', None)
   return redirect(url_for('index'))
```

运行应用程序并访问主页(确保设置应用程序的`secret_key`)。

```python
from flask import Flask, session, redirect, url_for, escape, request
app = Flask(__name__)
app.secret_key = 'any random string’
```

完整代码如下所示 - 

```python
from flask import Flask
from flask import render_template
from flask import request
from flask import make_response
from flask import Flask, session, redirect, url_for, escape, request



app = Flask(__name__)
app.secret_key = 'fkdjsafjdkfdlkjfadskjfadskljdsfklj'

@app.route('/')
def index():
    if 'username' in session:
        username = session['username']
        return '登录用户名是:' + username + '<br>' + \
                 "<b><a href = '/logout'>点击这里注销</a></b>"


    return "您暂未登录， <br><a href = '/login'></b>" + \
         "点击这里登录</b></a>"



@app.route('/login', methods = ['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))

    return '''
   <form action = "" method = "post">
      <p><input type ="text" name ="username"/></p>
      <p><input type ="submit" value ="登录"/></p>
   </form>
   '''

@app.route('/logout')
def logout():
   # remove the username from the session if it is there
   session.pop('username', None)
   return redirect(url_for('index'))


if __name__ == '__main__':
    app.run(debug = True)
```

输出将显示如下。点击链接“**点击这里登录**”。
![img](imge/WEB开发.assets/286160524_30991.png)

该链接将被引导至另一个界面。 输入’admin’。
![img](imge/WEB开发.assets/283160524_14904.png)

屏幕会显示消息“**登录用户名是:admin**”。如下所示 -
![img](imge/WEB开发.assets/322160525_75618.png)

## 14 重定向和错误

Flask类有重定向`redirect()`函数。调用时，它会返回一个响应对象，并将用户重定向到具有指定状态码的另一个目标位置。

`redirect()`函数的原型如下 -

```python
Flask.redirect(location, statuscode, response)
```

在上述函数中 -

- *location* 参数是响应应该被重定向的URL。
- *statuscode* 参数发送到浏览器的头标，默认为`302`。
- *response* 参数用于实例化响应。

以下状态代码是标准化的 -

- HTTP_300_MULTIPLE_CHOICES
- HTTP_301_MOVED_PERMANENTLY
- HTTP_302_FOUND
- HTTP_303_SEE_OTHER
- HTTP_304_NOT_MODIFIED
- HTTP_305_USE_PROXY
- HTTP_306_RESERVED
- HTTP_307_TEMPORARY_REDIRECT

默认状态码是`302`，这是表示’找到’页面。

在以下示例中，`redirect()`函数用于在登录尝试失败时再次显示登录页面。

```python
from flask import Flask, redirect, url_for, render_template, request
# Initialize the Flask application
app = Flask(__name__)

@app.route('/')
def index():
    return render_template('log_in.html')

@app.route('/login',methods = ['POST', 'GET'])
def login():
    if request.method == 'POST' and
        request.form['username'] == 'admin' :
        return redirect(url_for('success'))
    return redirect(url_for('index'))

@app.route('/success')
def success():
    return 'logged in successfully'

if __name__ == '__main__':
    app.run(debug = True)
```

Flask类具有带有错误代码的`abort()`函数。

```python
Flask.abort(code)
```

`code`参数使用以下值之一 -

- 400 - 对于错误的请求
- 401 - 用于未经身份验证
- 403 - 禁止
- 404 - 未找到
- 406 - 不可接受
- 415 - 用于不支持的媒体类型
- 429 - 请求过多

这里对上面的代码中的`login()`函数进行一些细微的修改。 如果要显示“Unauthourized”页面，而不是重新显示登录页面，请将其替换为中止(401)的调用。

```python
from flask import Flask, redirect, url_for, render_template, request, abort
app = Flask(__name__)

@app.route('/')
def index():
   return render_template('log_in.html')

@app.route('/login',methods = ['POST', 'GET'])
def login():
    if request.method == 'POST':
        if request.form['username'] == 'admin' :
            return redirect(url_for('success'))
        else:
            abort(401)
    else:
        return redirect(url_for('index'))

@app.route('/success')
def success():
    return 'logged in successfully'

if __name__ == '__main__':
    app.run(debug = True)
```

## 15 消息闪现

一个基于GUI好的应用程序需要向用户提供交互的反馈信息。 例如，桌面应用程序使用对话框或消息框，JavaScript使用`alert()`函数用于类似的目的。

在Flask Web应用程序中生成这样的信息消息很容易。 Flask框架的闪现系统使得可以在一个视图中创建一个消息并将其呈现在名为`next`的视图函数中。

Flask模块包含`flash()`方法。 它将消息传递给下一个请求，该请求通常是一个模板。

```python
flash(message, category)
```

在这里 - 

- *message* - 参数是要刷新的实际消息。
- *category* - 参数是可选的。 它可以是’错误’，’信息’或’警告’。

要从会话中删除消息，模板调用`get_flashed_messages()`函数。

```python
get_flashed_messages(with_categories, category_filter)
```

两个参数都是可选的。 如果收到的消息具有类别，则第一个参数是元组。 第二个参数对于仅显示特定消息很有用。

以下闪现模板中收到消息。

```html
{% with messages = get_flashed_messages() %}
   {% if messages %}
      {% for message in messages %}
         {{ message }}
      {% endfor %}
   {% endif %}
{% endwith %}
```

现在我们来看一个简单的例子，演示Flask中的闪现机制。 在下面的代码中，URL => “/”显示了到登录页面的链接，没有指定要发送的消息。

```python
@app.route('/')
def index():
    return render_template('index.html')
```

该链接引导用户显示登录表单的URL => “/login”。 提交时，login()函数验证用户名和密码，并相应地闪现“成功”或“错误”变量消息。

```python
@app.route('/login', methods = ['GET', 'POST'])
def login():
    error = None

    if request.method == 'POST':
        if request.form['username'] != 'admin' or \
            request.form['password'] != 'admin':
            error = 'Invalid username or password. Please try again!'
        else:
            flash('You were successfully logged in')
            return redirect(url_for('index'))
    return render_template('login.html', error = error)
```

如有错误，登录模板将重新显示并显示错误消息。

模板文件:*login.html* 代码如下 - 

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask示例</title>
</head>
   <body>

     <h1>登录</h1>
      {% if error %}
      <p><strong>Error:</strong> {{ error }}
      {% endif %}
      <form action = "/login" method ="POST">
         <dl>
            <dt>用户名:</dt>
            <dd>
               <input type = text name = "username" 
                  value = "{{request.form.username }}">
            </dd>
            <dt>密码:</dt>
            <dd><input type ="password" name ="password"></dd>
         </dl>
         <p><input type = submit value ="登录"></p>
      </form>

   </body>
</html>
```

如果登录成功，则在索引模板上闪现成功消息。以下代码保存在文件(*index.html*) - 

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask消息闪现</title>
</head>
   <body>


         {% with messages = get_flashed_messages() %}
          {% if messages %}
            <ul class=flashes>
            {% for message in messages %}
              <li>{{ message }}</li>
            {% endfor %}
            </ul>
          {% endif %}
        {% endwith %}

      <h1>Flask Message Flashing Example</h1>
      <p>您想要<a href = "{{ url_for('login') }}">
         <b>登录?</b></a></p>

   </body>
</html>
```

Flask消息闪现示例的完整代码如下所示 -

```python
from flask import Flask, flash, redirect, render_template, request, url_for
app = Flask(__name__)
app.secret_key = 'random string'

@app.route('/')
def index():
   return render_template('index.html')

@app.route('/login', methods = ['GET', 'POST'])
def login():
    error = None
    print(request.method)
    if request.method == 'POST':
        if request.form['username'] != 'admin' or \
            request.form['password'] != 'admin':
            error = 'Invalid username or password. Please try again!'
        else:
            #flash('您已成功登录')
            flash('You were successfully logged in')
            return redirect(url_for('index'))
    return render_template('login.html', error = error)

if __name__ == "__main__":
    app.run(debug = True)
```

执行上述代码后，您将看到如下所示的屏幕。
![img](imge/WEB开发.assets/677070507_14970.png)

当点击链接时，将会跳转到登录页面。输入用户名和密码 -
![img](imge/WEB开发.assets/263070518_87462.png)

点击**登录**按钮。 将显示一条消息“您已成功登录”。
![img](imge/WEB开发.assets/829070525_22677.png)

## 16 文件上传

在Flask中处理文件上传非常简单。 它需要一个enctype属性设置为`'multipart/form-data'`的HTML表单，将该文提交到指定URL。 URL处理程序从`request.files[]`对象中提取文件并将其保存到所需的位置。

每个上传的文件首先保存在服务器上的临时位置，然后再保存到最终位置。 目标文件的名称可以是硬编码的，也可以从`request.files [file]`对象的`filename`属性中获取。 但是，建议使用`secure_filename()`函数获取它的安全版本。

可以在Flask对象的配置设置中定义默认上传文件夹的路径和上传文件的最大大小。

| 变量                           | 说明                                      |
| ------------------------------ | ----------------------------------------- |
| app.config[‘UPLOAD_FOLDER’]    | 定义上传文件夹的路径                      |
| app.config[‘MAX_CONTENT_PATH’] | 指定要上传的文件的最大大小 - 以字节为单位 |

以下代码具有URL: `/upload` 规则，该规则显示`templates`文件夹中的`upload.html`文件，以及调用`uploader()`函数处理上传过程的URL => `/upload-file`规则。

`upload.html`有一个文件选择器按钮和一个提交按钮。

```html
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>Flask示例</title>
</head>
   <body>

     <form action = "http://localhost:5000/upload" method = "POST" 
         enctype = "multipart/form-data">
         <input type = "file" name = "file" />
         <input type = "submit" value="提交"/>
      </form>

   </body>
</html>
```

将看到如下截图所示 -
![img](imge/WEB开发.assets/442080555_84235.png)

选择文件后点击**提交**。 表单的post方法调用URL=> `/upload_file`。 底层函数`uploader()`执行保存文件操作。

以下是Flask应用程序的Python代码。

```python
from flask import Flask, render_template, request
from werkzeug import secure_filename
app = Flask(__name__)

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['file']
        print(request.files)
        f.save(secure_filename(f.filename))
        return 'file uploaded successfully'
    else:
        return render_template('upload.html')


if __name__ == '__main__':
    app.run(debug = True)
```

运行程序后，执行上面代码，选择一个图片文件，然后点击上传，得到以下结果 -
![img](imge/WEB开发.assets/814080557_38604.png)

## 17 扩展

Flask通常被称为微框架，因为核心功能包括基于Werkzeug的WSGI和路由以及基于Jinja2的模板引擎。 此外，Flask框架还支持cookie和会话以及Web助手，如JSON，静态文件等。显然，这对于开发完整的Web应用程序来说还不够。 这是为什么还要Flask扩展插件。 Flask扩展为Flask框架提供了可扩展性。

Flask有大量的扩展可用。 Flask扩展是一个Python模块，它为Flask应用程序添加了特定类型的支持。 Flask扩展注册表是一个可用扩展的目录。 所需的扩展名可以通过pip实用程序下载。

在本教程中，我们将讨论以下重要的Flask扩展 -

- **Flask Mail** − 为Flask应用程序提供SMTP接口
- **Flask WTF** − 添加了WTForms的渲染和验证
- **Flask SQLAlchemy** − 将SQLAlchemy支持添加到Flask应用程序中
- **Flask Sijax** − Sijax接口 - 使AJAX易于在Web应用程序中使用Python/jQuery库

每种类型的扩展通常提供有关其使用情况的大量文档。 由于扩展是一个Python模块，因此需要导入才能使用它。 Flask扩展名通常命名为`flask-foo`。导入语法如下，

```python
from flask_foo import [class, function]
```

对于低于`0.7`的Flask版本，还可以使用语法 -

```python
from flask.ext import foo
```

为此，需要激活兼容性模块。 它可以通过运行`flaskext_compat.py`来安装 - 

```python
import flaskext_compat
flaskext_compat.activate()
from flask.ext import foo
```

### 17.1 发送邮件

### 17.2 WTF

### 17.3 SQLite

### 17.4 QLAlchemy

### 17.5 Sijax

## 18 部署

开发服务器上的Flask应用程序只能在设置了开发环境的计算机上访问。 这是一种默认行为，因为在调试模式下，用户可以在计算机上执行任意代码。

如果禁用了调试，则通过将主机名设置为:`0.0.0.0`，可以使网络上的用户可以使用本地计算机上的开发服务器。

```python
app.run(host = ’0.0.0.0’)
Python
```

这样，您的操作系统会侦听所有公共IP，也就是说，所有请求都会被处理。

### 部署

要从开发环境切换到完整的生产环境，应用程序需要部署在真正的Web服务器上。 根据您的具体情况，可以使用不同的选项来部署Flask Web应用程序。

对于小型应用程序，可以考虑将其部署在以下任何托管平台上，所有这些平台都提供针对小型应用程序的免费计划。

- Heroku
- dotcloud
- webfaction

Flask应用程序可以部署在这些云平台上。 另外，可以在Google云平台上部署Flask应用程序。 Localtunnel服务允许您在本地主机上共享您的应用程序，而不会混淆DNS和防火墙设置。

如果您倾向于使用专用Web服务器来代替上述共享平台，则可以使用以下选项。

### mod_wsgi

`mod_wsgi`是一个Apache模块，它提供了一个用于在Apache服务器上托管基于Python的Web应用程序的WSGI兼容接口。

**安装mod_wsgi**

要从PyPi直接安装正式版本，可以运行 -

```python
pip install mod_wsgi
```

要验证安装是否成功，使用`start-server`命令运行`mod_wsgi-express`脚本 -

```shell
mod_wsgi-express start-server
```

它将在端口:8000上启动*Apache/mod_wsgi*。然后，可以通过将浏览器指向 -

```shell
http://localhost:8000/
```

**创建.wsgi文件**

应该有一个*yourapplication.wsgi* 文件。 该文件包含代码`mod_wsgi`，该代码在启动时执行以获取应用程序对象。 对于大多数应用程序，以下文件应该足够 -

```shell
from yourapplication import app as application
```

确保`yourapplication`和所有正在使用的库位于python加载路径上。

### **配置Apache**

需要告诉`mod_wsgi`，应用程序的位置。参考以下配置 - 

```shell
<VirtualHost *>
   ServerName example.com
   WSGIScriptAlias / C:\yourdir\yourapp.wsgi

   <Directory C:\yourdir>
      Order deny,allow
      Allow from all
   </Directory>

</VirtualHost>
```

### 独立的WSGI容器

有许多以Python编写的流行服务器，其中包含WSGI应用程序并提供HTTP服务。

- Gunicorn
- Tornado
- Gevent
- Twisted Web

## 19 FastCGI

FastCGI是Web服务器(如nginix，lighttpd和Cherokee)上Flask应用程序的另一个部署选项。

### 配置FastCGI

首先，需要创建FastCGI服务器文件，例如它的名称为:*yourapplication.fcgiC* 。

```python
from flup.server.fcgi import WSGIServer
from yourapplication import app

if __name__ == '__main__':
    WSGIServer(app).run()
```

nginx和较早版本的lighttpd需要明确传递一个套接字来与FastCGI服务器进行通信。需要将路径传递给WSGIServer的套接字。

```python
WSGIServer(application, bindAddress = '/path/to/fcgi.sock').run()
```

### 配置Apache

对于基本的Apache部署，*.fcgi* 文件将出现在您的应用程序URL中，例如`http://example.com/yourapplication.fcgi/hello/`。 有以下几种方法来配置应用程序，以便`yourapplication.fcgi`不会出现在URL中。

```shell
<VirtualHost *>
   ServerName example.com
   ScriptAlias / /path/to/yourapplication.fcgi/
</VirtualHost>
```

### 配置lighttpd

lighttpd的基本配置看起来像这样 -

```shell
fastcgi.server = ("/yourapplication.fcgi" => ((
   "socket" => "/tmp/yourapplication-fcgi.sock",
   "bin-path" => "/var/www/yourapplication/yourapplication.fcgi",
   "check-local" => "disable",
   "max-procs" => 1
)))

alias.url = (
   "/static/" => "/path/to/your/static"
)

url.rewrite-once = (
   "^(/static($|/.*))$" => "$1",
   "^(/.*)$" => "/yourapplication.fcgi$1"
)
```

请记住启用FastCGI，别名和重写模块。 该配置将应用程序绑定到`/yourapplication`。
