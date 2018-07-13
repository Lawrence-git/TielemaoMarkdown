# DJango2.0路由层-URLconf

[toc]

## 概述
* URL
    * 是Web服务的入口，就好像办事大厅有各个服务窗口一样；
    * 用户点击网页上的一些按钮或超链接，实际上就是通过浏览器发送请求到一个你指定的URL地址，然后被响应。
    * 有点像程序中的API。

* Django奉行DRY主义，提倡使用简洁、优雅的URL：
    * 可以不用`.html`、`.php`或`.cgi`之类后缀；
    * 尽量不要单独使用无序随机数字这样无意义的东西；
    * 让你随心所欲设计你的URL，不受框架束缚。

URL路由在Django项目中的体现就是urls.py文件，这个文件可以有很多个，但绝对不会在同一目录下。
实际上Django提倡项目有个根urls.py，各app下分别有自己的一个urls.py，既集中又分治，是一种解耦的模式。

新建一个Django项目，默认会自动会在项目根目录下创建一个`urls.py`文件，就是项目的根URL：

```python
"""BooksManage URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/2.0/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```
默认导入了url和admin，有一条指向admin后台的url路径。
建议注释掉后台路由或改掉默认后台url路径。

* URL配置(URLconf)就像Django 所支撑网站的目录树。
* 它的本质是URL与要为该URL调用的视图函数之间的映射表；
    * 对于客户端发来的某个URL调用哪一段逻辑代码对应执行。
    * 一般来说，一个路径对应一个视图函数。它并非一一对应！
    * 多个路径可以对应一个视图函数，但是一个路径，不能对应多个视图函数。

## urlpatterns
urls.py中默认就有urlpatterns，可以把它看作一个存放了映射关系的列表。
django2.0中常用的是path()方法，还可以使用re_path()方法来兼容1.x版本中的url()方法。

* 用法大致都是一样的，这些方法主要接收4个参数:
    * 2个是必須的：`regext`和`view`
    * 2个是可选的：`kwargs`和`name`

* regex(正则表达式）：
    * regex是正则表达式的通用缩写，可以用正则来匹配url地址。
    * 用户请求url地址，urls.py对urlpatterns列表中的每一项条目从头开始进行逐一对比，一旦遇到匹配项，立即执行该条目映射的视图函数或下级路由，其后的条目将不再继续匹配。
    * 所以，url路由的编写顺序非常重要！
    * regex不会去匹配GET或POST参数或域名。

* view(视图函数):
    * view指的是处理当前url请求的视图函数。
    * 当正则表达式匹配到某个条目时，自动将封装的HttpRequest对象作为第一个参数，正则表达式“捕获”到的值作为第二个参数，传递给该条目指定的视图view。
    * 如果是简单捕获，那么捕获值将作为一个位置参数进行传递，如果是命名捕获，那么将作为关键字参数进行传递。

* kwargs：
    * 任意数量的关键字参数可以作为一个字典传递给目标视图。

* name(别名)：
    * 对你的URL进行命名，让你能够在Django的任意处，尤其是模板内显式地引用它。
    * 这是一个非常强大的功能，相当于给URL取了个全局变量名，不会将url匹配地址写死。

## 实例

```python
from django.contrib import admin
from django.urls import path

from . import views

urlpatterns = [
    path('articles/2018/', views.special_case_2018),
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
]
```

注：

* 捕获一段url中的值，使用的是尖括号，而不是圆括号；
* 可以转换捕获到的值为指定类型，比如例子中的int。
    * 默认情况下，捕获到的结果保存为字符串类型，不包含/这个特殊字符；
* 匹配模式的开头可以不添加/，默认情况下，每个url都带一个最前面的/，共有的部分，不用特别写了。

匹配例子：
```
* /articles/2017/06/ 将匹配第三条，并调用views.month_archive(request, year=2017, month=6)；
* /articles/2018/匹配第一条，并调用views.special_case_2018(request)；
* /articles/2018将一条都匹配不上，因为它最后少了一个斜杠，而列表中的所有模式中都以斜杠结尾；
* /articles/2016/05/building-a-django-site/ 将匹配最后一个，并调用views.article_detail(request, year=2016, month=5, slug="building-a-django-site"。
```

## path转换器

默认情况下，Django内置下面的路径转换器：

* str：匹配任何非空字符串，但不含斜杠/，默认使用；
* int：匹配0和正整数，返回一个int类型;
* slug：可理解为注释、后缀、附属等概念，是url在最后的一部分解释性字符。
    * 该转换器匹配任何ASCII字符以及连接符和下划线；
* uuid：匹配一个uuid格式的对象。为了防止冲突，规定必须使用破折号，所有字母必须小写。
    * 例`0863561d3-9527-633c-b9b6-8a032e1565f0`。返回一个UUID对象；
* path：匹配任何非空字符串，重点是可以包含路径分隔符`/`。
    * 这个转换器可以帮助你匹配整个url而不是一段一段的url字符串。

## 自定义path转换器

写一个类，并包含下面的成员和属性：

* 类属性regex：一个字符串形式的正则表达式属性；
* to_python(self, value) 方法：
    * 用于将匹配到的字符串转换为你想要的那个数据类型，并传递给视图函数。
    * 如果转换失败，它必须弹出ValueError异常；
* to_url(self, value)方法：
    * 将Python数据类型转换为一段url的方法，to_python方法的反向操作。

例如，新建一个converters.py文件，与urlconf同目录，写个下面的类：

```python
class FourDigitYearConverter:
    regex = '[0-9]{4}'

    def to_python(self, value):
        return int(value)

    def to_url(self, value):
        return '%04d' % value
```

写完类后，在URLconf 中注册，并使用它，如下所示，注册了一个yyyy：

```python
from django.urls import register_converter, path

from . import converters, views

register_converter(converters.FourDigitYearConverter, 'yyyy')

urlpatterns = [
    path('articles/2018/', views.special_case_2018),
    path('articles/<yyyy:year>/', views.year_archive),
    ...
]
```

## 使用正则表达式

Django2.0的url虽然改‘配置’了，但它依然向老版本兼容。
而这个兼容的办法，就是用re_path()方法代替path()方法。
re_path()方法在骨子里，根本就是以前的url()方法，只不过导入的位置变了。

下面是一个例子，对比一下Django1.11时代的语法，有什么太大的差别？

```python
from django.urls import path, re_path

from . import views

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<slug>[\w-]+)/$', views.article_detail),
]
```

* 每个正则表达式前面的'r'是可选的但是建议加上。它告诉Python这个字符串是“原始的” —— 字符串中任何字符都不应该转义。
* 与path()方法不同的在于两点：
    * year中匹配不到10000等非四位数字，这是正则表达式决定的
    * 传递给视图的所有参数都是字符串类型。
        * 不像path()方法中可以指定转换成某种类型。在视图中接收参数时一定要小心。

## 命名组（有名分组）

如果需要获取URL中的一些片段，作为参数，传递给处理请求的视图函数，那么该怎么做呢？
答案是可以使用命名的正则表达式组来捕获URL中的值并以关键字参数传递给视图。

* 在Python的正则表达式中，命名组的语法是`(?P<name>pattern)`，
    * name是分组的名称，pattern是要匹配的模式。

使用有名分组，还可以解决因为视图函数中参数位置变动而导致页面显示混乱的情况。
无命名分组的时候，视图函数的形参名，可以随便定义。但是有命名分组，名字必须一一对应。
关键字参数在于，先赋值，再传参。所以视图函数，必须一一对应才行。

例：
```python
from django.urls import path,re_path

from app01 import views

urlpatterns = [
    re_path(r'^articles/2003/$', views.special_case_2003),
    re_path(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    re_path(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})/$', views.article_detail),
]
```

以上URL捕获的值作为关键字参数而不是位置参数传递给视图函数。

```
/articles/2005/03/ 请求将调用views.month_archive(request, year='2005', month='03')函数，而不是views.month_archive(request, '2005', '03')。

/articles/2003/03/03/ 请求将调用函数views.article_detail(request, year='2003', month='03', day='03')
```

在实际应用中，这意味你的URLconf 会更加明晰且不容易产生参数顺序问题的错误 —— 你可以在你的视图函数定义中重新安排参数的顺序。当然，这些好处是以简洁为代价；

* 命名组和非命名组：
    * 如果有命名参数，则使用这些命名参数，忽略非命名参数。
    * 否则，它将以位置参数传递所有的非命名参数。

## URLconf匹配请求URL中的哪些部分

请求的URL被看做是一个普通的Python字符串，URLconf将对其查找并进行匹配。
**注：进行匹配时将不包括GET或POST请求方式的参数以及域名。**
**捕获的参数作为字符串类型传递给视图。**

```
例:
在https://www.example.com/app/的请求中，URLconf匹配查找的是app/。
在https://www.example.com/app/?page=5的请求中，URLconf也将查找app/。
```

URLconf不会检查使用何种HTTP请求方法，所有请求方法POST、GET、HEAD等都将路由到同一个URL的同一个视图。
那么不同的请求在哪里进行处理呢？答案就是在URL路由的下一阶段，视图函数中才进行具体处理。

## django2.0版本的path

例：djiango 1.x版本的re_path

```python
urlpatterns = [  
    re_path('articles/(?P<year>[0-9]{4})/', year_archive),  
    re_path('article/(?P<article_id>[a-zA-Z0-9]+)/detail/', detail_view),  
    re_path('articles/(?P<article_id>[a-zA-Z0-9]+)/edit/', edit_view),  
    re_path('articles/(?P<article_id>[a-zA-Z0-9]+)/delete/', delete_view),  
]
```

思考：
第一个问题，函数 year_archive 中year参数是字符串类型的，因此需要先转化为整数类型的变量值，当然year=int(year) 不会有诸如如TypeError或者ValueError的异常。那么有没有一种方法，在url中，使得这一转化步骤可以由Django自动完成？

第二个问题，三个路由中article_id都是同样的正则表达式，但是你需要写三遍，当之后article_id规则改变后，需要同时修改三处代码，那么有没有一种方法，只需修改一处即可？

在Django2.0中，可以使用 path 解决以上的两个问题

这是一个简单的例子：

```python
from django.urls import path  
from . import views  
urlpatterns = [  
    path('articles/2003/', views.special_case_2003),  
    path('articles/<int:year>/', views.year_archive),  
    path('articles/<int:year>/<int:month>/', views.month_archive),  
    path('articles/<int:year>/<int:month>/<slug>/', views.article_detail),  
]
```

* 基本规则：
    * 使用尖括号`<>`从url中捕获值。
    * 捕获值中可以包含一个转化器类型（converter type），比如使用 `<int:name>` 捕获一个整数变量。
若果没有转化器，将匹配任何字符串，当然也包括了` / `字符。
    * 无需添加前导斜杠。

举例：

* `app01_urls.py`第一条规则使用path。

```python
urlpatterns = [
    path('articles/<int:year>/', views.article_year),  # article_year(request,year=value1)
    re_path(r'^articles/2003/$', views.special_year),  # special_year(request)
    re_path(r'^articles/(\d{4})/(\d{2})/$', views.article_month),  # article_month(request,value1,value2)
    # article_month(request,year=value1,month=value2,day=value3)
    re_path(r'^articles/(?P<year>\d{4})/(?P<month>\d{2})/(?P<day>\d{2})/$', views.article_day),

    path('login.html/', views.login, name="login_in"),
]
```
它和有命名分组类似，也是关键字传参。
不同的是，它做了类型转换，比如以上是转换成了int。

## 指定视图参数的默认值

小技巧来指定视图参数的默认值。 
其实就是和python函数中的传参道理是一样的。
示例：

`URLconf(urls.py)`如下：

```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^blog/$', views.page),
    url(r'^blog/page(?P<num>[0-9]+)/$', views.page),
]
```

`View (views.py)`如下：

```python
def page(request, num="2"):
    # Output the appropriate page of blog entries, according to num.
    ...
```

上面的例子中，两个URL模式指向同一个视图views.page。
但是第一个模式不会从URL中捕获任何值。 
如果第一个模式匹配，`page()`函数将使用num参数的默认值"2"。 
如果第二个模式匹配，`page()`将使用捕获的num值。

## 分发（路由转发）

在1个Django项目里面有多个APP目录，大家共有一个url容易造成混淆。
这时可以使用路由分发让每个APP的拥有自己单独的url，方便以后的维护管理。
做解藕，不将所有的url放到一个py文件里面，根据应用名来进行拆分。
也就是在每个app里，各自创建一个`urls.py`路由模块，然后从根路由出发，将app所属的url请求，全部转发到相应的`urls.py`模块中。

* `urls.py`中的官方说明：

```python
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
```

   * 1、首先导入include方法；
   * 2、然后添加URL模式的URL；

路由转发使用的是`include()`方法，所以需要导入，它的参数是转发目的地路径的字符串，路径以圆点分割。

每当Django 遇到`include()`（来自`django.conf.urls.include()`）时，它会去掉URL中匹配的部分并将剩下的字符串发送给include的URLconf做进一步处理，也就是转发到二级路由去。

* 在urls.py导入include方法：
`from django.urls import include`

例：

```python

'''
At any point, your urlpatterns can “include” other URLconf modules. This
essentially “roots” a set of URLs below other ones.
'''

from django.urls import path,re_path,include
from app01 import views

urlpatterns = [
   re_path(r'^admin/', admin.site.urls),
   re_path(r'^blog/', include('blog.urls')),
]
```

* 添加独立的url文件:
例如在app01目录下创建app01_urls.py，将urls.py相关的内容复制过去

```python
from django.urls import path,re_path,include
from app01 import views
urlpatterns = [
    re_path(r'^articles/(\d{4})/$', views.article_year),  # article_year(request,分组匹配的值)
    re_path(r'^articles/2003/$', views.special_year),  # special_year(request)
    re_path(r'^articles/(\d{4})/(\d{2})/$', views.article_month),  # article_month(request,value1,value2)
    # article_month(request,year=value1,month=value2,day=value3)
    re_path(r'^articles/(?P<year>\d{4})/(?P<month>\d{2})/(?P<day>\d{2})/$', views.article_day),
]
```

* 修改urls.py，删除多余的代码。

```python
from django.contrib import admin
from django.urls import path,re_path,include
from app01 import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index),
    re_path('^$', views.index),
    re_path('^index/$', views.index),  # index(request)
    path('app01/', include('app01.app01_urls')),
    # 注意：app01后面，必须有斜杠，否则页面无法访问。
]
```

访问原来的url: `http://127.0.0.1:8000/articles/2003/`，提示404。
因为路由分发了，所以访问时，必须加上应用名。
正常访问url：`http://127.0.0.1:8000/app01/articles/2003/`页面访问正常。

## 反向解析

在使用Django 项目时，一个常见的需求是获得URL 的最终形式，以用于嵌入到生成的内容中（视图中和显示给用户的URL等）或者用于处理服务器端的导航（重定向等）。
    * 就好比像是拿到一块拼图碎片，在嵌入到拼图板中。

做为开发者，你也不会希望使要硬编码这些URL（费力、不可扩展且容易产生错误）,因为这样容易导致一定程度上产生过期的URL。你会希望设计一种与URLconf 毫不相关的专门的URL 生成机制或者直接不用硬编码的方式。

* 在需要URL 的地方，对于不同层级，Django 提供不同的工具用于URL 反查：
    * 在`urls.py`中起一个别名name（`name=自定义的别名`)
    * 在`templates`模板中：使用`url '变量名(别名)' `语法。
    * 在`views.py`中导入`reverse`方法：`from django.urls import reverse()`
    * 在更高层的与处理Django模型实例相关的代码中：使用`get_absolute_url()`方法。(也就是在模型model中)

### urls和templates中使用反向解析

例：以登陆页面为例
* 原`urls.py`配置

```python
from django.contrib import admin
from django.urls import path,re_path
from app import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('login/', views.login),
]
```

* 原`views.py`中相关配置

```python
def login(request):
    return render(request, 'login.html')
```

* 原templates目录下`login.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="/login/" method="post">
        <label>用户名</label>
        <input type="text" name="user"/>
        <label>密码</label>
        <input type="password" name="pwd" />
        
        <input type="submit" value="登录">
    </form>
</body>
</html>
```

现发生需求变更，业务线的URL发生了更改：
要求从原`http://127.0.0.1:8000/login/`改为`http://127.0.0.1:8000/login.html/`。
我们当然可以手动将相关的login改为login.html，
在这个简单例子里大至就只需要改动urls中的`login\`和html中的form表单action中的`\login\`两处为`login.html\`就完成了。
但如果是真实生产复杂的环境，某些大量重复出现的URL就不便了。
从这一个简单例子可看出之前硬编码（写死）的一个URL，后期发生需求变更URL要改变的时候，特别不利于维护。
由鉴于此，可以使用反向解析的方法去解决。

以上为例：
* 修改后的`urls.py`部分

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('login.html/', views.login, name="login"),
    # 为login.html添加了一个别名叫login
]
```
上述修改为`login.html`url填加了一个别名叫`login`。
别名去代指url。此时`login`对应的值是路径`login.html/`。

* 修改后的`login.html`文件部分，用到一个语法，来引用`url`变量。

```html
    <form action={% url 'login' %} method="post">
        {% csrf_token %}
        <label>用户名</label>
        <input type="text" name="user"/>
        <label>密码</label>
        <input type="password" name="pwd" />

        <input type="submit" value="登录">
    </form>
```

更改部分`<form action={% url 'login' %} method="post">`。
它表示从url文件(urls.py)中，调用变量login。利用render将页面渲染，返回给浏览器。
重新访问url：`http://127.0.0.1:8000/login.html/`

![](https://www.tielemao.com/wp-content/uploads/2018/07/django_url.jpg)

使用控制台查看html代码，发现action的属性，就是login.html。说明已经被后端给渲染出来了。
这就是反向解析，路径会变，但是别名不会变。别名是随着路径的变动而变动的。
如此以后再有改动url，只需在`urls.py`中改动一处就可以了，方便维护。
推荐以后写页面，使用别名（反向解析）。

### 视图函数中使用反向解析
html文件中使用反向解析我们知道了，那么视图函数中又如何使用反向解析呢？
还是以上述为例，只不过这次我们以增加登录验证函数和成功跳转到首页为例。

* urls.py中新增部分：

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index, name="index"),
    path('login/', views.login, name="login"),
]
```
给`index/`同样设置一个别名，后面视图函数会使用到。

* login视图函数修改部分：

```python
from django.shortcuts import render,HttpResponse,redirect
from django.urls import reverse

# 导入HttpResponse方法，这可以直接发送字符串给浏览器，简单测试模拟页面用
# 导入redirect方法，它用于控制跳转页面
# 导入reverse方法，用于反向解析(传入别名变量作为参数）

def index(request):
    return HttpResponse('<h1>首页欢迎你，简单模拟首页页面用<h1>')


def login(request):
    if request.method == "POST":
        # 当请求类型为post时
        username = request.POST.get("user")
        # request.post.get可以拿到form表单中以关键字(name=)命名的变量值，
        # 也就是拿到用户输入的用户名和密码
        password = request.POST.get("pwd")
        # 简单模拟数据库验证，通过验证时跳转到首页
        if username == 'tielemao' and password == '123':
            # 硬编码方式： return redirect('/index/')
            # 使用反向解析如下，
            # reverse方法用于将index别名解析回/index/
            return redirect(reverse("index"))
    return render(request, 'login.html')
```

效果如图：
![](https://www.tielemao.com/wp-content/uploads/2018/07/login_index.gif)

之后不论在`urls.py`中`index/`怎么改变，只要别名指向它，通过反向解析的视图函数永远拿到的是最新的。

## 名称空间(Namespace)

* 命名空间（英语：Namespace）是表示标识符的可见范围。
    * 一个标识符可在多个命名空间中定义，它在不同命名空间中的含义是互不相干的。
    * 在一个新的命名空间中可定义任何标识符，它们不会与任何已有的标识符发生冲突，因为已有的定义都处于其它命名空间中。

* 为什么引入命名空间？
    * 由于name（别名）没有作用域，Django在反解URL时，会在项目全局顺序搜索，当查找到第一个name指定URL时，立即返回。
    * 我们在开发项目时，会经常使用name属性反解出URL，当不小心在不同的app的urls中定义相同的name时，可能会导致URL反解错误，
   * 为了避免这种事情发生，引入了命名空间。

* URL命名空间可以保证反查到唯一的URL，即使不同的app使用相同的URL名称。
* 第三方应用始终使用带命名空间的URL是一个很好的做法。

类似地，它还允许你在一个应用有多个实例部署的情况下反查URL。 
换句话讲，因为一个应用的多个实例共享相同的命名URL，命名空间提供了一种区分这些命名URL 的方法。

实现命名空间的做法很简单，在urlconf文件中添加`app_name = 'tielemao'`和`namespace='author-login'`这种类似的定义。

例：有2个应用，app01和app02。
如果url控制层定义的name值一致，那么就会出现冲突。
为了解决这个问题，需要用到名称空间。

修改urls.py里面的路由分发，使用关键字`namespace`声明命名空间

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index, name="index"),
    re_path('^$', views.index),
    re_path('^', include('app01.app01_urls',namespace="app01")),
]
```

* `app01_urls.py`，注册自定义转化器。并应用在article_year视图函数上。
注意：如果有命名空间，必须要声明app_name，否则报错

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
#导入url_converter模块中的FourDigitYearConverter类
from app01.url_converter import FourDigitYearConverter

#必须导入register_converter方法
from django.urls import path,re_path,include,register_converter

from app01 import views

#注册方法FourDigitYearConverter,定义别名为yyyy
register_converter(FourDigitYearConverter,"yyyy")

#注意：如果有命名空间，必须要声明app_name，否则报错
app_name = 'app01'

urlpatterns = [
    #使用自定义转换器
    path('articles/<yyyy:year>/', views.article_year),  
    # article_year(request,year=value1)
    re_path(r'^articles/2003/$', views.special_year),  
    # special_year(request)
    re_path(r'^articles/(\d{4})/(\d{2})/$', views.article_month),  
    # article_month(request,value1,value2)
    # article_month(request,year=value1,month=value2,day=value3)
    re_path(r'^articles/(?P<year>\d{4})/(?P<month>\d{2})/(?P<day>\d{2})/$', views.article_day),

    path('login/', views.login, name="login_in"),
]
```

* `urls.py`文件：

```python
from django.conf.urls import include, url

urlpatterns = [
    url(r'^app01/', include('appo1.urls')),
]
```

此时，`appo1.urls`中定义的URL将具有应用名称空间app01。

注意：如果有命名空间，必须要声明app_name，否则报错。
在include的URLconf模块中设置与urlpatterns属性相同级别的app_name属性。
必须将实际模块或模块的字符串引用传递到`include()`，而不是urlpatterns本身的列表。

* app下的`urls.py1`声明命名空间的语法为 `app_name = 命名空间名`
* 视图函数使用命名空间的语法为`reverse("命名空间名:url别名")`
* 模板(templates)html文件中使用命名空间的语法为`{% url '命名空间名:url别名' %}`

## 自定义错误页面

当Django找不到与请求匹配的URL时，或者当抛出一个异常时，将调用一个错误处理视图。
错误视图包括400、403、404和500，分别表示请求错误、拒绝服务、页面不存在和服务器错误。
它们分别位于：

```
handler400 —— django.conf.urls.handler400。
handler403 —— django.conf.urls.handler403。
handler404 —— django.conf.urls.handler404。
handler500 —— django.conf.urls.handler500。
```

这些值在根URLconf中设置，在其它app中的二级URLconf中设置这些变量无效。
Django有内置的HTML模版，用于返回错误页面给用户，但正常都会使用自己定义的友好错误页面。

步骤如下：

* 在根URLconf中额外增加下面的条目：

`URLconf(urls.py)`如下：

```python
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^blog/$', views.page),
    url(r'^blog/page(?P<num>[0-9]+)/$', views.page),
]

# 增加的条目
handler400 = views.bad_request
handler403 = views.permission_denied
handler404 = views.page_not_found
handler500 = views.page_error
```

* 在views.py文件中相对增加四个处理视图：
```
def page_not_found(request):
    return render(request, '404.html')


def page_error(request):
    return render(request, '500.html')


def permission_denied(request):
    return render(request, '403.html')


def bad_request(request):
    return render(request, '400.html')
```

* 根据自己的需求，创建404.html、400.html等四个页面文件，就可以了。

【end】
2018-7-2

参考引用：刘江的博客及教程部分
http://www.liujiangblog.com/blog/17/
http://www.liujiangblog.com/course/django/134