# Django视图层
[toc]

## 概述

Django的视图层，通俗的说用户请求从路由层urls.py处理后就来到了这一层，往下就是视情况走model层（数据库）或直接返回templates模板中的html页面，又或者直接用函数方法快捷处理了，可以说是一个很重要也很贴近编程代码的一层。

基本在此模块我们所写的代码方式和python中一样，主要以def函数为主。
也因此，虽然简称为视图，习惯称为视图函数，它的本质上就是一个写满密密麻麻的函数的python模块。
此外，它主要做的是接收路由分发过来的web请求并且返回web响应。
响应的内容一般最终会是一个经过数据处理的HTML网页；
也包括了重定向、404错误，XML文档或图像等任何东西。
但是，无论视图本身是个什么处理逻辑，最好都返回某种响应。

**比喻**

是不是感觉有点像在饭店里看着菜单点单，服务员上各种菜式给你一样。
没错，这里我们到店点单，菜单就相当于接受我们最初请求的路由层，服务员送到后厨就相当于是送进了视图层，后厨根据这些请求（下单点的菜式）对应返回响应（按菜谱做好菜）并让服务员将结果上到你桌上。（用户接收到响应）

通常我们约定将视图放置在项目或应用程序目录中，且命名为`views.py`文件。

##  简单的视图实例

* 写一个返回当前日期和时间的HTML文档视图：

`urls.py`文件中指定访问地址部分：

```python
urlpatterns = [
     path('time/', views.current_datatime,)
]
```

`views.py`文件中添加：

```python
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body><h1>当前时间为：%s <h1></body></html>" % now
    return HttpResponse(html)
```

上述代码做了些什么呢？

* 它首先，从django.http模块导入了HttpResponse类，以及Python的datetime库。
* 接着，定义了一个名字为自定义的`current_datetime`的视图函数。
* 最后，它返回了一个HttpResponse对象，其中包含生成的显示当前时间的HTML页面。

注：
* 每个视图函数都接收一个HttpRequest对象作为第一位置参数，一般取名为request，你可以取别的名字，但这不符合约定俗成，最好不要那么做。情形就类似学习面向对象的时候，一个类的方法通常会接收一个默认叫做self的第一位置参数一样。

* 视图函数的名称没有强制规则，但尽量不要和Python及Django内置的各种名称重名，并且尽量精确地反映出它的功能。（顾名思义）

最终效果为用户访问`/time/`页面显示如图：
![](https://www.tielemao.com/wp-content/uploads/2018/07/current_datetime.jpg)

## 返回错误页面

* Django中的错误页面一般我们不用，不过做为学习和开发测试阶段，玩一下也是可以的。
* Django中主要使用HttpResponse对象来简单返回HTTP错误代码。
    * 因HttpResponse的许多子类对应着一些常用的HTTP状态码。

标示一个错误，可以直接返回那些子类中的一个实例，而不用普通的HttpResponse也可以。
像下例：

* `urls.py`文件代码相关部分：

```python
urlpatterns = [
     path('test/', views.test_error),
]
```

* `views.py`文件代码相关部分：

```python
from django.http import HttpResponse, HttpResponseNotFound

# Create your views here.

def test_error(request):
    # 测试用，可定义一个foo函数，返回ture或false的布尔值进行观测。
    if foo:
        return HttpResponseNotFound('<h1>404 :Page not found </h1>')
    else:
        return HttpResponse('<h1>欢迎光临</h1>')
```

Django为404错误提供了一个特化的子类HttpResponseNotFound。
一些不太常见和常用的状态码就没有这个特殊待遇了。毕竟404太常打交道了。

访问效果如下，可通过控制台看到的确是404状态。
![](https://www.tielemao.com/wp-content/uploads/2018/07/HttpResponseNotFound.jpg)

类似地，其他一些常用的特定状态HttpResponse子类如下：

* HttpResponseRedirect：返回Status 302，用于URL重定向，需要将重定向的目标地址作为参数传给该类。
* HttpResponseNotModified：返回Status 304，用于指示浏览器用其上次请求时的缓存结果作为页面内容显示。
* HttpResponsePermanentRedirect: 返回Status 301，永久重定向。
* HttpResponseBadRequest：返回Status 400，请求内容错误。
* HttpResponseForbidden：返回Status 403 ， 禁止访问错误。
* HttpResponseNotAllowed ： 返回Status 405，用不允许的方法（Get、Post、Head等）访问本页面。
* HttpResponseServerError： 返回Status 500， 服务器内部错误，比如无法处理的异常等。

也可以向HttpResponse的构造器传递HTTP状态码，来创建你想要的任何状态码的返回类。 
例，修改上述例子代码相关部分：

```python
from django.http import HttpResponse, HttpResponseNotFound

def test_error(request):
    return HttpResponse(status=303)
```
如图，访问效果：
![](https://www.tielemao.com/wp-content/uploads/2018/07/HttpResponse303.jpg)

### HTTP 404异常

`class django.http.Http404`是一个Django内置的异常类。
可以在你认为需要的地方弹出它，Django会捕获它，
并且带上HTTP404错误码返回你当前app的标准错误页面或者自定义的错误页面。

例，同样接上述例子修改部分代码进行测试：

```python
from django.http import Http404
from django.shortcuts import render

def test_error(request):
    # 测试用，正常可定义一个执行体，进行try和except捕获异常，进行观测。
    if 1:
        raise Http404('测试错误信息 by tielemao')
    else:
        return HttpResponse('欢迎')
```

效果如图：
![](https://www.tielemao.com/wp-content/uploads/2018/07/Http404.jpg)

上面是我为了方便测试用的简单代码，下面有一个例子使用try和except对照：

```python
from django.http import Http404
from django.shortcuts import render
from polls.models import Poll

def detail(request, poll_id):
    try:
        p = Poll.objects.get(pk=poll_id)
    except Poll.DoesNotExist:
        raise Http404("Poll does not exist")
    return render(request, 'polls/detail.html', {'poll': p})
```

为了在Django返回404时显示自定义的HTML，
可以创建一个名为404.html的HTML模板，并将其放置在模板树的顶层。 
当在`settings.py`文件中DEBUG设置为False时，此模板将被自动使用。

当DEBUG为True时，可以向Http404提供消息，
它将显示在标准的内置404调试模板中，可以使用这些消息进行调试。
上面例子的效果图就是DEBUG为True时的。可见自定义的消息。
当然这些调试信息都只会在开发测试阶段显示，
正式线上环境时DEBUG必然是为了安全（不能暴露信息）要设置为False的。

##  Djangoy请求和响应对象

Django 是围绕着 Request 与 Response 进行处理，也就是无外乎“求”与“应”。

当请求一个页面时，Django 把请求的 metadata 数据包装成一个 HttpRequest 对象，然后 Django 加载合适的 view 方法，把这个 HttpRequest 对象作为第一个参数传给 view 方法。
任何 view 方法都应该返回一个 HttpResponse 对象。

![](https://www.tielemao.com/wp-content/uploads/2018/07/HttpRequest.jpg)

Django在django.shortcuts模块中，提供了很多快捷方便的类和方法。
下面着重介绍相对很重要，使用频率很高的一些类和方法。

### HttpRequest对象

每当一个用户请求发送过来，Django将HTTP数据包中的相关内容，打包成为一个HttpRequest对象，并传递给每个视图函数作为第一位置参数，也就是request，供我们调用。
HttpRequest对象中包含了非常多的重要的信息和数据，应该熟练掌握它。
* 类定义：`class HttpRequest[source]`

#### HttpRequest主要属性
django将请求报文中的请求行、首部信息、内容主体封装成 HttpRequest 类中的属性。 
除了特殊说明的之外，其他均为只读。

* HttpRequest.scheme （协议）
    * 表示请求的协议种类，http或https；
    * 字符串类型。
    
* HttpRequest.body （正文主体）
    * 表示原始HTTP请求的正文；
    * 对于处理非HTML形式的数据非常有用，例：图像（二进制）、XML等；
    * bytes类型。
    
* HttpRequest.path （路径）
    * 表示当前请求页面的完整路径（不包括协议名和域名）；
    * 常用于判断执行某操作时，不通过则返回用户先前浏览的页面；
    * 字符串类型。
    
* HttpRequest.path_info (路径信息）
    * 在某些Web服务器配置下，主机名后的URL部分被分成脚本前缀部分和路径信息部分。
    * `path_info` 属性将始终包含路径信息部分，不论使用的Web服务器是什么。
    * 使用它代替path可以让代码在测试和开发环境中更容易地切换。
    例：如果应用的WSGIScriptAlias设置为`/minfo`，那么`HttpRequest.path`等于`/music/bands/the_beatles/` ，而`HttpRequest.path_info`为`/minfo/music/bands/the_beatles/`。
    
* HttpRequest.method (请求方法)
    * 表示请求使用的HTTP方法。
    * 默认为大写。
    * 常用于处理常规的form表单数据。根据请求的方法不同，在视图中判断执行不同的代码。
    * 字符串类型。

* HttpRequest.GET （GET请求）
    * 包含HTTP GET的所有参数；
    * 类似于字典的对象；
    * 属于`django.http.QueryDict`对象的实例，能调用QueryDict对象的方法。

* HttpRequest.POST （POST请求）
    * 包含所有HTTP POST请求的参数及表单数据；
    * 类似于字典的对象；
    * 属于`django.http.QueryDict`对象的实例，能调用QueryDict对象的方法。
    * 不包含上传文件的数据，文件信息包含在FILES属性中；
    * POST请求可以为空且QueryDict实例依然会创建，因此，视图函数中检查请求使用的是否是POST方法，不应使用`if request.POST`而应使用`if request.method == "POST"`;
    * 键值对是多个的时候（如checkbox类型的input标签，select标签）需使用`request.POST.getlist("hobby")`之类。
    
* HttpRequest.encoding (编码方式)
    * 表示提交的数据的编码方式；
    * 如为None则表示使用`DEFAULT_CHARSET`设置；
    * 此属性为可写，可以修改它来改变表单数据的编码，使随后的属性访问（例GET或POST）将使用新的编码方式。

* HttpRequest.COOKIES
    * 包含所有Cookie信息的字典。
    * 键和值都为字符串。
    * 可以以类似字典类型的方式，在cookie中读写数据。
    * 注意cookie是不安全的，不要写入或透露敏感重要信息。

* HttpRequest.FILES (文件上传)
    * 类似字典的对象；
    * 包含所有上传的文件数据；
    * 每个键为`<input type="file" name="" />`中的name属性值；
    * 每个值为一个上传的文件`UploadedFile`。
    * Django中实现文件上传，靠的是这个属性。
    * 如果请求的方法是POST且请求的`<from>`中带有`enctype="multipart/form-data"`属性，那么FILES将包含上传的文件的数据。 否则，FILES将为一个空的类似于字典的对象，属于被忽略、无用的情形。

* HttpRequest.META (头部信息）
    * 包含所有HTTP头部信息的字典。 可用的头部信息取决于客户端和服务器。
    下面是一些示例：
```
    CONTENT_LENGTH —— 请求正文的长度（以字符串计）。
    CONTENT_TYPE —— 请求正文的MIME类型。
    HTTP_ACCEPT —— 可接收的响应Content-Type。
    HTTP_ACCEPT_ENCODING —— 可接收的响应编码类型。
    HTTP_ACCEPT_LANGUAGE —— 可接收的响应语言种类。
    HTTP_HOST —— 客服端发送的Host头部。
    HTTP_REFERER —— Referring页面。
    HTTP_USER_AGENT —— 客户端的user-agent字符串。
    QUERY_STRING —— 查询字符串。
    REMOTE_ADDR —— 客户端的IP地址。想要获取客户端的ip信息，就在这里！
    REMOTE_HOST —— 客户端的主机名。
    REMOTE_USER —— 服务器认证后的用户，如果可用。
    REQUEST_METHOD —— 表示请求方法的字符串，例如"GET" 或"POST"。
    SERVER_NAME —— 服务器的主机名。
    SERVER_PORT —— 服务器的端口（字符串）。
```
以上只是比较重要和常用的，还有很多未列出。
从上面可以看到，除CONTENT_LENGTH和CONTENT_TYPE之外，请求中的任何HTTP头部键转换为META键时，都会将所有字母大写并将连接符替换为下划线最后加上HTTP_前缀。所以，一个叫做X-Bender的头部将转换成META中的HTTP_X_BENDER键。

* 可自定义的属性
    * Django不会自动设置下面这些属性，而是由你自己在应用程序中设置并使用它们。
    * HttpRequest.current_app
        * 表示当前app的名字。
        * url模板标签将使用其值作为`reverse()`方法的current_app参数。
    * HttpRequest.urlconf
        * 设置当前请求的根URLconf，用于指定不同的url路由进入口，
        * 覆盖settings中的`ROOT_URLCONF`设置。
        * 将它的值修改为None，可以恢复使用`ROOT_URLCONF`设置。

* 由中间件设置的属性
    * Django的contrib应用中包含的一些中间件会在请求上设置属性。
    * HttpRequest.session （会话）
        * SessionMiddleware中间件：一个可读写的，类似字典的对象，表示当前会话。
        * 我们要保存用户状态，回话过程等等，靠的就是这个中间件和这个属性。
    * HttpRequest.site （站点）
        * CurrentSiteMiddleware中间件：`get_current_site()`方法返回的Site或RequestSite的实例，代表当前站点是哪个。
        * Django是支持多站点的，如果你同时上线了几个站点，就需要为每个站点设置一个站点id。
    * HttpRequest.user （用户）
        * AuthenticationMiddleware中间件：表示当前登录的用户的`AUTH_USER_MODEL`的实例;
        * 这个模型是Django内置的Auth模块下的User模型。如果用户当前未登录，则user将被设置为AnonymousUser的实例。
        * 可以使用is_authenticated方法判断当前用户是否合法用户，如下所示：
        
```python
if request.user.is_authenticated:
    ... # Do something for logged-in users.
else:
    ... # Do something for anonymous users.
```

#### HttpRequest主要方法

* `HttpRequest.get_host()[source]`
    * 根据`HTTP_X_FORWARDED_HOST`和`HTTP_HOST`头部信息获取请求的原始主机。 
    * 如果这两个头部没有提供相应的值，则使用`SERVER_NAME`和`SERVER_PORT`。
    * 例："127.0.0.1:8080"
    * 注：当主机位于多个代理的后面，`get_host()`方法将会失败。
    * 解决办法之一是使用中间件重写代理的头部，如下面的例子：
    
```python
from django.utils.deprecation import MiddlewareMixin

class MultipleProxyMiddleware(MiddlewareMixin):
    FORWARDED_FOR_FIELDS = [
        'HTTP_X_FORWARDED_FOR',
        'HTTP_X_FORWARDED_HOST',
        'HTTP_X_FORWARDED_SERVER',
    ]

    def process_request(self, request):
        """
        Rewrites the proxy headers so that only the most
        recent proxy is used.
        """
        for field in self.FORWARDED_FOR_FIELDS:
            if field in request.META:
                if ',' in request.META[field]:
                    parts = request.META[field].split(',')
                    request.META[field] = parts[-1].strip()
```

* `HttpRequest.get_port()[source]`
    * 使用META中`HTTP_X_FORWARDED_PORT`和`SERVER_PORT`的信息返回请求的始发端口。
    
* `HttpRequest.get_full_path()[source]`
    * 返回包含完整参数列表的path。例如： `/music/bands/the_beatles/?print=true`
    
* `HttpRequest.build_absolute_uri(location)[source]`
    * 返回location的绝对URI形式。 
    * 如果location没有提供，则使用`request.get_full_path()`的值。
    * 例如：`"https://example.com/music/bands/the_beatles/?print=true"`
    * 注：不鼓励在同一站点混合部署HTTP和HTTPS，如果需要将用户重定向到HTTPS，最好使用Web服务器将所有HTTP流量重定向到HTTPS。
    
* `HttpRequest.get_signed_cookie(key, default=RAISE_ERROR, salt='', max_age=None)[source]`
    * 从已签名的Cookie中获取值，如果签名不合法则返回django.core.signing.BadSignature。
    * 可选参数salt用来为密码加盐，提高安全系数。 max_age参数用于检查Cookie对应的时间戳是否超时。

范例：

```python
>>> request.get_signed_cookie('name')
'Tony'
>>> request.get_signed_cookie('name', salt='name-salt')
'Tony' # assuming cookie was set using the same salt
>>> request.get_signed_cookie('non-existing-cookie')
...
KeyError: 'non-existing-cookie'
>>> request.get_signed_cookie('non-existing-cookie', False)
False
>>> request.get_signed_cookie('cookie-that-was-tampered-with')
...
BadSignature: ...
>>> request.get_signed_cookie('name', max_age=60)
...
SignatureExpired: Signature age 1677.3839159 > 60 seconds
>>> request.get_signed_cookie('name', False, max_age=60)
False
```

* `HttpRequest.is_secure()[source]`
    * 如果使用的是Https，则返回True，表示连接是安全的。
    
* `HttpRequest.is_ajax()[source]`
    * 如果请求是通过XMLHttpRequest生成的，则返回True。
    * 这个方法的作用就是判断，当前请求是否通过ajax机制发送过来的。

* 从HttpRequest实例读取文件数据的方法：
``` python   
1、HttpRequest.read(size=None)[source]
2、HttpRequest.readline()[source]
3、HttpRequest.readlines()[source]
4、HttpRequest.xreadlines()[source]
5、HttpRequest.iter()
```
    * 可以将HttpRequest实例直接传递到XML解析器，例如ElementTree：
```python
import xml.etree.ElementTree as ET
for element in ET.iterparse(request):
    process(element)
```

### HttpResponse 对象
Request 和 Response 对象起到了服务器与客户机之间的信息传递作用。
Request 对象用于接收客户端浏览器提交的数据，而 Response 对象的功能则是将服务器端的数据发送到客户端浏览器。

与由Django自动创建的HttpRequest 对象相比，
HttpResponse 对象由程序员创建.你创建的每个视图负责初始化实例,
填充并返回一个 HttpResponse。

**每个 View 方法必须返回一个 HttpResponse 对象。**
HttpResponse 类是在django.http模块中定义的。

主要介绍下面三种方法：

* `HttpResponse()` 传递字符串
* `render()` 结合一个给定的模板和一个给定的上下文字典，并返回一个渲染后的 HttpResponse 对象。
* `redirect()`重定向

#### `HttpResponse()`
以字符串的形式传递给页面。
一般地，你可以通过给 HttpResponse 的构造函数传递字符串表示的页面内容来构造HttpResponse 对象：

```python
>>> response = HttpResponse("Welcome to tielemao.com.")
>>> response = HttpResponse("Text only, please.", mimetype="text/plain")
```

如果想要增量添加内容, 你可以把response当作filelike对象使用：

```python
response = HttpResponse()
response.write("<p>Welcome to tielemao.com.</p>")
response.write("<p>Here's another paragraph.</p>")
```

 也可以给 HttpResponse 传递一个 iterator 作为参数，而不用传递硬编码字符串。 
 如果你使用这种技术，下面是需要注意的一些事项：

* iterator 应该返回字符串。
* 如果 HttpResponse 使用 iterator 进行初始化，就不能把 HttpResponse 实例作为 filelike 对象使用。这样做将会抛出异常。

最后，再说明一下，HttpResponse 实现了 `write() `方法，可以在任何需要 filelike 对象的地方使用 HttpResponse 对象。

#### `render()`
* 语法
`render(request, template_name, context=None, content_type=None, status=None, using=None)[source]`

* 用处：结合一个给定的模板和一个给定的上下文字典，返回一个渲染后的HttpResponse对象。

* 必要参数：
    * request：视图函数处理的当前请求，封装了请求头的所有数据，其实就是视图参数request。
    * template_name：要使用的模板的完整名称或者模板名称的列表。
        * 如果是一个列表，将使用其中能够查找到的第一个模板。

* 可选参数：
    * context：添加到模板上下文的一个数据字典。默认是一个空字典。
        * 可以将需要提供给模板的数据以字典的格式添加进去。
        * 使用Python内置的`locals()`方法，可以方便的将函数作用域内的所有变量一次性添加。
    * content_type：用于生成的文档的MIME类型。 默认为DEFAULT_CONTENT_TYPE设置的值。
    * status：响应的状态代码。 默认为200。
    * using：用于加载模板使用的模板引擎的NAME。

**范例：**

* `index.html`文件：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <h1>首页欢迎你，简单模拟首页页面用<h1>
        <p>当前时间为：{{ time }}</p>
</body>
</html>
```
* `views.py`相关视图函数部分

```python
def index(request):
    now = datetime.datetime.now().ctime()
    print(now)
    return render(request, 'index.html', {"time": now})
```

效果就是用户访问首页的时候，视图函数从templates中找到index.html渲染并返回给用户。
其中获取当前时间的数据是使用了相关的语法，
类似字典，time在html中充当了key，now变量为value并最终渲染到key占据的位置上。
可以想像成在图书馆里提前替未到的另一半占位置的一对恋人。

#### `redirect()`

* 语法
`redirect(to, permanent=False, args, *kwargs)[source]`

根据传递进来的url参数，返回HttpResponseRedirect。
通俗的说就是跳转或叫重定向到传入的地址。
可以想像为水上乐园的滑道，本来你排着队是要滑到水池下的，
但滑的中途整个管道被人为的折弯了N度滑向了另外一个泳池，这就是重定向。

* 参数to：
    * 一个模型：将调用模型的`get_absolute_url()`函数，反向解析出目的url；
    * 视图名称：可能带有参数：`reverse()`将用于反向解析url；
    * 一个绝对的或相对的URL：将原封不动的作为重定向的目标位置。

默认情况下是临时重定向，如果设置permanent=True将永久重定向。

**范例：**

* `views.py`相关视图函数部分

```python
from django.shortcuts import redirect,render
from django.urls import reverse

def login(request):
    if request.method == "POST":
        # 当请求类型为post时
        username = request.POST.get("user")
        password = request.POST.get("pwd")
        # 简单模拟数据库验证，通过验证时跳转到首页
        if username == 'tielemao' and password == '123':
            # 硬编码方式： return redirect('/index/')
            # 使用反向解析如下，
            # reverse方法用于将index别名解析回/index/
            return redirect(reverse("index"))
    return render(request, 'login.html')
```

临时重定向HTTP状态码是302，永久重定向HTTP状态码是301，两者主要区别是在搜索引擎方面。
上面的例子为用户get请求访问login的时候，返回的是login.html页面。
而当用户通过form表单填写了用户名和密码提交post请求并通过验证之后，
就会被redirect跳转（重定向）到reverse反向解析的index页面上。

效果如gif图：
![](https://www.tielemao.com/wp-content/uploads/2018/07/login302index.gif)

* 301和302的区别。
    * 共同点：
        * 301和302状态码都表示重定向；
        * 浏览器在拿到服务器返回的这个状态码后会自动跳转到一个新的URL地址，这个地址可以从响应的Location首部中获取
        * 用户看到的效果就是他输入的地址A瞬间变成了另一个地址B
    * 不同点：
        * 301表示旧地址A的资源已经被永久地移除了（这个资源不可访问了），搜索引擎在抓取新内容的同时也将旧的网址交换为重定向之后的网址；
        * 302表示旧地址A的资源还在（仍然可以访问），这个重定向只是临时地从旧地址A跳转到地址B，搜索引擎会抓取新的内容而保存旧的网址。 
        * 对SEO来说302好于301。
   * 重定向原因：
       * 网站调整（如改变网页目录结构）；
       * 网页被移到一个新地址；
       * 网页扩展名改变(如应用需要把.php改成.Html或.shtml)。
         这种情况下，如果不做重定向，则用户收藏夹或搜索引擎数据库中旧地址只能让访问客户得到一个404页面错误信息，访问流量白白丧失；
       * 某些注册了多个域名的网站，也需要通过重定向让访问这些域名的用户自动跳转到主站点等。

### `get_object_or_404()`

* 语法
`get_object_or_404(klass, args, *kwargs)[source]`

这个方法，非常有用，请一定熟记。
常用于查询某个对象，找到了则进行下一步处理，如果未找到则给用户返回404页面。

在后台，Django其实是调用了模型管理器的`get()`方法，只会返回一个对象。
不同的是，如果`get()`发生异常，会引发Http404异常，从而返回404页面，而不是模型的DoesNotExist异常。

* 必需参数：
    * klass：要获取的对象的Model类名或者Queryset等；
    *  `**kwargs`: 查询的参数，格式应该可以被`get()`接受。

**范例：**

* 1.从MyModel中使用主键1来获取对象：
    
```python
from django.shortcuts import get_object_or_404

def my_view(request):
    my_object = get_object_or_404(MyModel, pk=1)
```

这个示例等同于：

```python
from django.http import Http404

def my_view(request):
    try:
        my_object = MyModel.objects.get(pk=1)
    except MyModel.DoesNotExist:
        raise Http404("No MyModel matches the given query.")
```

* 2.除了传递Model名称，还可以传递一个QuerySet实例：

```python
queryset = Book.objects.filter(title__startswith='M')
get_object_or_404(queryset, pk=1)
```

上面的示例不够简洁，因为它等同于：

`get_object_or_404(Book, title__startswith='M', pk=1)`

但是如果你的queryset来自其它地方，它就会很有用了。

* 3.还可以使用Manager。 如果你自定义了管理器，这将很有用：

`get_object_or_404(Book.dahl_objects, title='Matilda')`

* 4.还可以使用related managers：

```python
author = Author.objects.get(name='Roald Dahl')
get_object_or_404(author.book_set, title='Matilda')
```

与`get()`一样，如果找到多个对象将引发一个MultipleObjectsReturned异常。

### `get_list_or_404()`

* 语法
`get_list_or_404(klass, args, *kwargs)[source]`

这其实就是get_object_or_404多值获取版本。

在后台，返回一个给定模型管理器上`filter()`的结果，并将结果映射为一个列表，如果结果为空则弹出Http404异常。

* 必需参数：
    * klass：获取该列表的一个Model、Manager或QuerySet实例。
    * `**kwargs`：查询的参数，格式应该可以被`filter()`接受。

**范例：**

下面的示例从MyModel中获取所有发布出来的对象：

```python
from django.shortcuts import get_list_or_404

def my_view(request):
    my_objects = get_list_or_404(MyModel, published=True)
```

这个示例等同于：

```python
from django.http import Http404

def my_view(request):
    my_objects = list(MyModel.objects.filter(published=True))
    if not my_objects:
        raise Http404("No MyModel matches the given query.")
```

关于raise：
在Python中，要想引发异常，最简单的形式就是输入关键字raise，后跟要引发的异常的名称。

【end】
参考引用：
http://www.liujiangblog.com/course/django/137
http://www.liujiangblog.com/course/django/138

