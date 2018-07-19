Django与Ajax
===

@toc

学习Ajax之前，我们先来将json的知识重温一遍，毕竟和json关系挺密切的。

# 什么是JSON

* JSON是JavaScript Object Notation的缩写
* 意思是指JavaScript对象表示法
* JSON是轻量级的文本数据交换格式
* JSON独立于语言和平台
* JSON相比于XML具有自我描述性，更易理解
* 通俗的说，JSON就是一串有特定符号标注而成的字符串
	* `{}`双括号表示python中的字典，其它语言中的对象
	* `[]`中括号表示python中的列表，其它语言中的数组
	* 属性（键）或值使用双引号`" "`括起来，特别是键（字符串），不能使用单引号或没有。
	* `：`冒号表示分隔，后者是前者（键）的值，值可以是字符串、数字、数组乃至另一个对象

## Python与JSON

如图，json对应python的数据结构表：
![python2json]($res/python2json.jpg)

如图，序列化与反序列化：
![Serialization]($res/Serialization.png)

序列化Serialization是一种将对象以一连串字节描述的过程；
反序列化Deserialization是一种将这些字节重建成一个对象的过程。

* JavaScript中关于JSON对象和字符串转换的两个方法：
	* `JSON.parse()`用于将一个JSON字符串转换为JavaScript对象（反序列化）
	* `JSON.stringify()`用于将JavaScript值转换为JSON字符串（序列化）

* Python中序列化的方法
	* `json.dumps()`在内存中进行序列化
	* `json.loads()`在内存中进行反序列化
	* 以上两者（多了个s复数)主要用于网络传输和多个数据与文件打交道
	* `json.dump()`在文件中进行序列化
	* `json.load()`在文件中进行反序列化
	* 主要用于一个数据直接存放在文本里，直接和文件打交道

## XML与JSON

**XML和JSON都使用结构化方法来标记数据。**
它们可以说是一对冤家了，毕竟JSON作为新生代，它的出现是为了取代老前辈XML的。
而且Ajax的缩写中最后的x代表的也是XML，但是事到如今，大家都会喜欢传输JSON多于XML。
如果JSON早在Ajax命名的时候就火热，说不定Ajax的缩写就该是Ajaj了。突然有点萌感。
那么json和xml相比，它有哪些更让人喜爱的地方呢？

**json和xml相比：**
* 书写简单，一目了然，结构化更为清晰
* 更小巧
* 描述更易理解
* 由于更小巧所以网络传输数据将减少更多流量从而加快速度
* JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码

# AJAX简介

* 全称：**Asynchronous Javascript And XML**
* 翻译过来就是"异步的javascript和XML"
* 即使用Javascript语言，与服务器能进行异步交互，传输的数据为XML
* 当然现在传输的数据不只是XML了，更多的是使用JSON传输
* AJAX不是新的编程语言，而是一种使用现有标准的新方法，所以不要陷入误区了。
	* AJAX是基于现有的Internet标准，并且联合使用它们：
	* XMLHttpRequest 对象 (异步的与服务器交换数据)
	* JavaScript/DOM (信息显示/交互)
	* CSS (给数据定义样式)
	* XML或JSON (作为转换数据的格式)
* AJAX是一种用于创建快速动态网页的技术。
* 最大的优点是在不用重新加载整个页面的情况下，便可以与服务器交换数据并更新部分页面内容。
	* 传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。
* 且以上过程给用户的感受是在不知不觉中便完成了请求和响应的，体验好。
* AJAX不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。

* 特点
	* 异步请求
	* 局部更（刷）新。

* 比喻
	* 局部刷新：可以想像成动画中一个镜头，当中只有人物的口型在动，而不是整副画面都变成了另一副镜头；
	* 异步请求：可以想像办事大厅中有好几个窗口空着不需要排队，同时有几个人同时在等窗口的服务人员返回结果给他们，当然返回的时间也是有长有短的；

## 同步与异步

* 同步交互：客户端发出一个请求后，需要等待服务器响应结束后，才能发出第二个请求；
* 异步交互：客户端发出一个请求后，无需等待服务器响应结束，就可以发出第二个请求。

## 常见应用场景

* 搜索引擎根据用户输入的关键字，自动提示搜索建议列表。

![ajax-part-brush]($res/ajax-part-brush.jpg)

* 注册时候用户名的查重。（会有提示用户名己经被占用之类）

![ajax-async-request]($res/ajax-async-request.jpg)

* 登录时候用户名或密码输入错误。（同样会有提示）

![ajax-async-request2]($res/ajax-async-request2.jpg)

## 工作原理

![ajax-work]($res/ajax-work.jpg)

当文件框发生了输入变化时-->使用AJAX技术向服务器发送一个请求;
服务器把查询到的结果响应给-->浏览器;
浏览器再把后端返回的结果-->局部展示出来。

*  整个过程中页面没有刷新，只是刷新页面中的局部位置而已！
*  当请求发出后，浏览器还可以进行其他操作，无需等待服务器的响应！

流程图
![Ajax-flow-chart]($res/Ajax-flow-chart.jpg)
 
* 1、客户端也就是浏览器触发事件（比如按纽上的鼠标单击事件）进行Ajax异步操作；
* 2、创建新的XMLHttpRequest对象，这是ajax的核心（需要着重学习）
* 3、通过send发送请求到server端
* 4、服务器端接收请求，并进行处理（比如从数据库中获取数据）
* 5、返回处理的结果
	* 这个结果可以是XML文档
	* 也可以是josn字符串
	* 一般情况下josn就可以处理大部分的结果、而且相对的比较好操作
	* 处理Ajax请求一般返回httpResponse()（json字符串对象）
* 6、客户端去接收服务器传回来的结果，并且通过javascript进行你想要的处理在渲染到浏览器上面（局部刷新）。

## 实例

### 简单的ajax请求
效果：当点击页面上的［三体］按钮时，在按钮的下方呈现出［消灭人类暴政，世界属于三体］字样。

以一个Django新项目为例，其中的相关代码如下：

`urls.py`中添加index和test路由:
```python
from django.contrib import admin
from django.urls import path
from app import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index, name="index"),
    path('test/', views.test，name="test"),
]
```

`views.py`中添加视图函数index和test：

```python
from django.shortcuts import render, HttpResponse

# Create your views here.

def index(request):
    return render(request, "index.html")

def test(request):
    return HttpResponse("消灭人类暴政，世界属于三体")
```

`templates`模板中添加index.html文件，内容如下：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>测试用</title>
    <link href="/static/css/bootstrap.css" rel="stylesheet">
</head>
<body>
    <div class="col-md-3">
        <button class="btn btn-primary" id="book" title="点我试试">三体</button>
        <p class="con"></p>
    </div>
    <script src="/static/js/jquery.js"></script>
    <script src="/static/js/bootstrap.js"></script>
    <script>
        $("#book").click(function () {
            //当对id为book的按钮进行点击操作时触发发送ajax请求
            $.ajax({
                url: "/test/", //请求要走的url路由
                type: "get", //默认请求就是get
                success: function (data) { //必須要有一个形参，例如data来接收后端发过来的响应体
                    console.log(data); //在浏览器控制台打印显示data
                    $(".con").text(data); //将data发送给con标签，显示文本内容
                }
            })
        })
    </script>
</body>
</html>
```
注意，上面的html页面中使用了bootsrap和jquery，这不是重点。此时访问页面的效果如下：
![ajax-demo01]($resource/ajax-demo01.gif)

上例的ajax请求代码中，success表示请求成功，并拿到响应体之后，执行的（函数）动作！
data是用来接收响应体的数据。
此时，后端服务器中我们定义了test视图函数返回的是一个字符串给data。
所以在局部（dom操作p标签）将"消灭人类暴政，世界属于三体"这句话给刷新显示出来了。

### ajax加法运算（get请求）

用户在网页输入两个整数，浏览器通过ajax传输两个加数到后端，后端计算出结果后返回给用户。


