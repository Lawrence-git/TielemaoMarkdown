# Django简介

[toc]

## MVC与MTV模型

* MVC百度百科：全名Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。

* 通俗解释：一种文件的组织和管理形式！不要被缩写吓到了，这其实就是把不同类型的文件放到不同的目录下的一种方法，然后取了个高大上的名字。当然，它带来的好处有很多，比如前后端分离，松耦合等等。

* MTV: 有些WEB框架觉得MVC的字面意思很别扭，就给它改了一下。view不再是HTML相关，而是主业务逻辑了，相当于控制器。html被放在Templates中，称作模板，于是MVC就变成了MTV。这其实就是一个文字游戏，和MVC本质上是一样的，换了个名字和叫法而已，换汤不换药。

### MVC
* Web服务器开发领域里著名的MVC模式，将web应用分为以下三层：
    * 模型(Model)
    * 视图(View)
    * 控制器(Controller)
* 以上三层之间以一种插件式的、松耦合的方式连接在一起：
    * 模型负责业务对象与数据库的映射(ORM)
    * 视图负责与用户的交互(页面)
    * 控制器接受用户的输入调用模型和视图完成用户的请求。
* 其示意图如下所示：

![](https://www.tielemao.com/wp-content/uploads/2018/06/MVC.png)


### MTV

Django的MTV模式本质上和MVC是一样的，也是为了各组件间保持松耦合关系，只是定义上有些许不同，Django的MTV分别是指：

    M 代表模型（Model）： 负责业务对象和数据库的关系映射(ORM)。
    T 代表模板 (Template)：负责如何把页面展示给用户(html)。
    V 代表视图（View）：   负责业务逻辑，并在适当时候调用Model和Template。

除了以上三层之外，还需要一个URL分发器，它的作用是将一个个URL的页面请求分发给不同的View处理，View再调用相应的Model和Template，MTV的响应模式如下所示：

![](https://www.tielemao.com/wp-content/uploads/2018/06/MTV.png)

用户通过浏览器向我们的服务器发起一个请求(request)，这个请求会去访问视图函数，（如果不涉及到数据调用，那么这个时候视图函数返回一个模板也就是一个网页给用户），视图函数调用模型，模型去数据库查找数据，然后逐级返回，视图函数把返回的数据填充到模板中，最后返回网页给用户。

* Django 的MTV模型组织
![](https://www.tielemao.com/wp-content/uploads/2018/06/django_frame.jpg)


## Django项目实例

### 安装
python3.5、pip3及pycharm专业版可自行安装。
例：windows cmd命令行自动安装Pypi提供的最新版本。
`pip3 install django`

### 配置环境变量
成功安装Djangio后，如有需要，可以将python的Scripts目录加入到系统环境变量中，以便调用django-admin命令。

配置完成后，可直接在cmd任一路径下运行django-admin help命令测试安装和配置完成：
![](https://www.tielemao.com/wp-content/uploads/2018/06/django-admin_help.jpg)

### 创建django项目和应用
* 在windows cmd命令行界面下，使用diango提供的命令创建diango项目如下：
    * `django-admin startproject mysite`
    * 其中mysite是项目名称，可自行替换成你想建立的项目名。
* 而在该项目下创建应用的命令是：
    * `python manage.py startapp blog`
    * 其中blog为app，应用名称，可自行替换成你想建立的应用名称。
* 启动django项目的命令为：
    * `python manage.py runserver 8080`
    * runserver 默认为本机（127.0.0.1）,后面跟的8080为端口号，可根据实际环境替换。
* 这样一个简单的原始的django就启动起来了，我们到浏览器输入127.0.0.1:8080实际上访问的就是django的这个项目，如图：
![](https://www.tielemao.com/wp-content/uploads/2018/06/django_web.jpg)


一般开发使用pycharm（IDE）操作是点击file-new project，选择Django栏目；
右侧选择项目所在路径，选择项目使用的python版本环境（可选虚拟环境），
注意Location中选择项目路径的同时所选的目录也就是项目的名称，
More Settings栏可设置模板文件夹名，web应用名称，勾选自动创建相关web应用文件夹等。
点击右下方的Create按钮创建。

![](https://www.tielemao.com/wp-content/uploads/2018/06/pycharm_new_Django.jpg)

Django自动生成类似下面的目录结构：
![](https://www.tielemao.com/wp-content/uploads/2018/06/Django_Create_init.jpg)
* 和项目同名的文件夹中存放的是
    * settings.py 配置文件；
    * urls.py url路由文件；
    * wsgi.py 网络通信接口模块；
* templates模板目录下为空，此目录主要用于存放各个html模板文件。
* 项目根目录下的manage.py文件为django项目的管理主程序，工具等。
* 各个应用目录（如我这边的创建的应用名为app01）下存放主要有：
    * views.py 为处理业务逻辑；
    * tests.py 为单元测试；
    * modes.py 为处理数据库；
* 推荐在项目根目录下自行建立起一个static的静态文件，用于存放css，js，img，html等静态文件。

* 在每个Django项目中可以包含多个APP，相当于一个大型项目中的分系统、子模块、功能部件等等，相互之间比较独立，但也可以有联系。
    * 所有的APP共享项目资源。

### 编写路由（url控制器）
路由由urls文件进行处理，功能是将浏览器输入的url映射到相应的(views)业务处理逻辑。
由于和业务处理逻辑相关，也就是和views相关，所以在文件开头就需要先导入对应app的views.py文件。

例：没做增添之前的urls.py(包括了官方注释）

```python
"""tielemao URL Configuration

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

增添路由条目后：

```python
from django.contrib import admin
from django.urls import path

# 需先导入对应的app目录中的views文件
from app01 import views

urlpatterns = [
    # admin为后台管理的路由，一般不会暴露出来，注释掉居多
    # path('admin/', admin.site.urls),
    # 自己新增的路由条目，前半部分表示路径的为正则表达式，后半部分为对应的业务逻辑函数
    path('index/', views.index),
]
```

### 编写业务处理逻辑（views视图函数）
urls.py中增添的路由条目中对应了相应的自己命名的业务逻辑函数，
也就是接下来你就需要为此在相应的views.py文件增添上相应的视图函数。

原始的views.py文件：

```python
from django.shortcuts import render

# Create your views here.
```

增添相应函数后：

```python
from django.shortcuts import render

# 导入HttpResponse模块
from django.shortcuts import HttpResponse

# Create your views here.

# request参数按规范必須有，类似self的默认存在，名字可以改，但不建议。
# 它里面封装了用户请求的所有内容。
def index(request):
    # 可以print打印request.POST或request.GET来查看到请求
    # print(request.POST)
    # print(request.GET)

    # 正常是不能直接返回字符串，必須使用Django提供的HttpResponse
    # 这个类封装起来就可以返回字符串了，这是Django的规则，不是python的。
    return HttpResponse("hello world!")
```

通过上面两个简单的步骤，将index这个url指向了views里的index（）函数，它接收用户请求，并返回一个“hello world”字符串。
我们就可以启动web服务演示一下了。

### 运行web服务
* 命令行方式：`python manage.py runserver 127.0.0.1:8000`
* pycharm中可以通过在工具栏中找到编辑配置文件的选项，快速进行设置host和port后再点击绿色三角形进行运行：
![](https://www.tielemao.com/wp-content/uploads/2018/06/pycharm_StartWeb2.jpg)

![](https://www.tielemao.com/wp-content/uploads/2018/06/pycharm_StartWeb.jpg)

运行效果：
![](https://www.tielemao.com/wp-content/uploads/2018/06/Pycharm_StartWeb3.jpg)

在浏览器中访问http://127.0.0.1:8000
![](https://www.tielemao.com/wp-content/uploads/2018/06/DjangoDebug404.jpg)

此时会出现404的错误信息，因为此时我们访问的地址并不是index/,在开发过程中，Django给出的这些错误信息很重要，仔细阅读方便排错，但一旦正式上线生产环境，就一定要关掉如此详细的调错信息功能，常见的错误就自己另写html报错页面。

在地址栏中输入http://127.0.0.1:8000/index/，访问才会出现己设置好的正常显示的hello world！
![](https://www.tielemao.com/wp-content/uploads/2018/06/Django_index.jpg)

### 返回HTML
上面例子返回给用户的是一个字符串，真正的web应用肯定是不会这样做的，通常返回的都应该是一个HTML文件给用户。那么，我们写如下showtime函数和time.html的HTML文件，做为一个用户访问获取
当前时间的功能例子：

* urls.py代码如下：

```python
from django.contrib import admin
from django.urls import path

# 需先导入对应的app目录中的views文件
from app01 import views

urlpatterns = [
    # admin为后台管理的路由，一般不会暴露出来，注释掉居多
    path('admin/', admin.site.urls),
    # 自己新增的路由条目，前半部分表示路径的为正则表达式，后半部分为对应的业务逻辑函数
    path('index/', views.index),
    # 新增用户访问time/路由,获取当前时间函数
    path('time/', views.showtime),
]
```

* views.py代码如下，增加一个showtime函数：

```python
from django.shortcuts import render

# 导入HttpResponse模块
from django.shortcuts import HttpResponse

# 导入时间模块
import datetime

# Create your views here.

# request参数按规范必須有，类似self的默认存在，名字可以改，但不建议。
# 它里面封装了用户请求的所有内容。
def index(request):
    # 正常是不能直接返回字符串，必須使用Django提供的HttpResponse
    # 这个类封装起来就可以返回字符串了，这是Django的规则，不是python的。
    return HttpResponse("hello world!")

def showtime(request):
    now=datetime.datetime.now()
    ctime=now.strftime("%Y-%m-%d %X")
    return render(request, "time.html", {"ctime":ctime})
```

request,它是一个对象。
当中存储了请求信息，比如请求路径，请求方式，GET数据，POST数据...等等。
必须要接收一个request参数。
当你想返回一个html文件时，不是使用HttpResponse方法，而是使用render方法来渲染（打包）。
不过本质上render最终还是使用了HttpResponse方法发送byte字节给浏览器的。

* 模板templates目录下，新建一个time.html:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    {# 由两个大括号括起来里面加个变量名，相当于是字典的键名，是django用于占位输出的语法 #}
    <h3>当前时间：{{ ctime }}</h3>
</body>
</html>
```

访问效果如下：
![](https://www.tielemao.com/wp-content/uploads/2018/06/showtime.jpg)

### settings.py设置模板文件夹
settings.py文件中有TEMPLATES变量，它是一个列表，列表中又存放了一个字典，其中一个键值对
`'DIRS':[os.path.join(BASE_DIR, 'templates')]`
效果就是默认设置了模板目录是使用默认的项目文件夹下的templates目录。
如果有特殊需要修改的就是在此改动。
另外django有一个好处，代码更改之后，一般无需重启web服务，它会自动加载最新代码。

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')]
        ,
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

### 使用静态文件
将html文件返回给用户还不够，前端三大块，HTML、CSS、JS还有各种插件等，完整齐全才是一个好看的页面。在django中，一般将静态文件放在static目录中。接下来，在项目根目录下新建一个static目录。
同时，我还在此目录下建立起js，css，img子目录和相关文件，如图：

![](https://www.tielemao.com/wp-content/uploads/2018/06/Django_static.jpg)

static这个静态目录名和Django默认设置的静态目录名别名一致，
在settings.py中可找到相关设置项，就是在结尾处再添加上新的一行表示告诉Django静态目录的路径：

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.0/howto/static-files/

STATIC_URL = '/static/'
# STATIC_URL表示的是引用别名（指针），不是具体的目录
# 可以改成你想指定的名字，但是在相应的引用地方必須和它对应到

# 增加以下一段表示设置静态目录的路径
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)
# 真实目录名不要在html中写死，而是写成别名引用，
# 如此，目录名就算有改动也只需改动此处即可。
# 另外，由于此行是一个元组，别忘了后面还需加个逗号。
```

同理，在html文件中引用静态文件，例如jquery.js文件如下：

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    {# 由两个大括号括起来里面加个变量名，相当于是字典的键名，是django用于占位输出的语法 #}
    <h3>当前时间：{{ ctime }}</h3>
    <script src="/static/js/jquery.js"></script>
</body>
</html>
```

主要看`<script src="/static/js/jquery.js"></script>`这一行，里面的路径并没有写死，而是使用了`static/`来代指了真实的静态目录。

### 接收用户发送的数据（get和post请求）
至此，我们做到了将一个要素齐全的HTML文件返还给了用户浏览器。
但这还不够，因为web服务器和用户之间还没有动态交互。
下面我们来设计一个login页面，上面建立一个表单，让用户输入用户名和密码，提交给login
这个url，服务器将接收到这些数据。

login.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        {# 注意Django中有一个跨站请求保护机制，所以需要加以下一行 #}
        {% csrf_token %}
        用户名：<input type="text" name="user" />
        密码：<input type="password" name="pwd" />
        <input type="submit" value="提交" />
    </form>
</body>
</html>
```

* 这其中牵涉到一个csrf的防护机制
    * CSRF百度百科：CSRF（Cross-site request forgery）跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。

* 假如没有加`{% csrf_token %}`这一行防护，运行时将会报如下图的错误：
![](https://www.tielemao.com/wp-content/uploads/2018/06/django_CSRF.jpg)

* urls.py中`urlpatterns`添加路由条目：
`path('login/', views.login),`

* views.py中添加login函数：
```python
def login(request):
    if request.method=="POST":
        username = request.POST.get("user", None)
        password = request.POST.get("pwd", None)
        print("用户名：", username,"密码", password)
    return render(request, "login.html")
```
此逻辑处理将会在pycharm中可以看到用户输入的用户名密码。

运行效果如下：
html页面效果：
![](https://www.tielemao.com/wp-content/uploads/2018/06/django_login.jpg)

pycharm后端效果：
![](https://www.tielemao.com/wp-content/uploads/2018/06/django_post.jpg)

pycharm中可以看到提交后的post请求数据后端都获取到了。

### 返回动态页面
我们收到了用户的数据，但返回给用户的依然是个静态页面，通常页面会根据用户的数据，进行处理后在返回给用户。
django采用自己的模板语言，类似jinja2，可根据提供的数据，替换掉HTML中的相应部分。

例：views.py文件修改如下：

```python
# 创建一个用户信息表，预设了两个数据，将返回给浏览器展示给用户
user_list = [
    {"user":"tielemao", "pwd":"12345"},
    {"user":"LiLei", "pwd":"abc123"},
]

def login(request):
    if request.method=="POST":
        username = request.POST.get("user", None)
        password = request.POST.get("pwd", None)
        temp = {"user":username, "pwd":password}
        user_list.append(temp)
    return render(request, "login.html", {"data":user_list})
# render接收的第三个参数是后台返回给浏览器的数据，一个字典。
# data是字典的键，是你在login.html中自定义的指针名字，对应引用值。
```

而login.html相应修改：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="" method="post">
        {# 注意Django中有一个跨站请求保护机制，所以需要加以下一行 #}
        {% csrf_token %}
        用户名：<input type="text" name="user" />
        密码：<input type="password" name="pwd" />
        <input type="submit" value="提交" />
    </form>

    <h2>用户列表</h2>
    <table border="1">
        <thead>
            <th>用户名</th>
            <th>密码</th>
        </thead>
        <tbody>
            {% for line in data %}
            <tr>
                <td>{{ line.user }}</td>
                <td>{{ line.pwd }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
</body>
</html>
```
login.html中利用for循环将data(引用）迭代填入数据到表格。
访问页面并输入一些数据测试效果如下：
![](https://www.tielemao.com/wp-content/uploads/2018/06/django_dynamic.jpg)

效果就是用户列表会随着提交的数据而发生变化，算是一个简单的动态页面， 和用户的交户过程。

### 使用数据库
上面我们虽然和用户交互得很好，但并没有保存任何数据，页面一旦关闭，或服务器重启，一切都将回到原始状态。
使用数据库是正常最常见的，django通过自带的ORM框架操作数据库，并且自带轻量级的sqlite3数据库。这次我们先来使用sqlite数据库来演示，后面再学习详细使用mysql。
首先是注册app，不进行这一步的话django不会知道该给哪个app创建表。

* 在settings.py中注册你的app:

```python
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01.apps.App01Config',
]
```

例，因为我之前执行了创建应用的命令，django2.06版本自动在创建应用的同时就帮我注册了app,就是上面代码中的`app01.apps.App01Config`。如果重复注册一个app01，会在运行`python manage.py makemigrationsw`命令的时候报错`django.core.exceptions.ImproperlyConfigured: Application labels aren't unique, duplicates: app01`

然后在settings中，配置数据库相关的参数，这次使用自带的sqlite，不需要修改。

```python
# Database
# https://docs.djangoproject.com/en/2.0/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

* 编辑models.py，也就是MTV中的M。

```python
from django.db import models

# Create your models here.

# 固定继承models.Model这个类
class UserInfo(models.Model):
    id = models.AutoField(primary_key=True)
    user = models.CharField(max_length=32)
    pwd = models.CharField(max_length=32)
```
以上创建了三个字段，分别保存id，用户名和密码。

* 通过命令创建数据库的表：
    * `python manage.py makemigrations`
        * 注：migration译作迁移
        * 注意此时运行完此命令只是翻译了了sql语句，还没有真正将sql语句实行到数据库中
```cmd
E:\Django\tielemao>python manage.py makemigrations
Migrations for 'app01':
  app01\migrations\0001_initial.py
    - Create model UserInfo
```

    * `python manage.py migrate`
        * 此命令执行后才是真正在相应的数据库中创建好了表。
        * 同时还会将diango一些认为要创建的表创建上。

```cmd
E:\Django\tielemao>python manage.py migrate
Operations to perform:
  Apply all migrations: admin, app01, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying app01.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying sessions.0001_initial... OK
```

* 使用sqlite3作为数据库的话，现在可以在项目根目录下看到有`db.sqlite3`文件
    * 可以使用查看sqlite文件的软件连接查看该数据库验证。
    * 例如使用navicat连接sqlite：
    ![](https://www.tielemao.com/wp-content/uploads/2018/06/natcat_sqlite3.jpg)
    ![](https://www.tielemao.com/wp-content/uploads/2018/06/django-sqlite3.jpg)

* 修改views.py中的业务逻辑如下：

```python

from django.shortcuts import render
# 导入models文件
from app01 import models

def login(request):
    if request.method=="POST":
        username = request.POST.get("user", None)
        password = request.POST.get("pwd", None)
        # 添加数据到数据库
        models.UserInfo.objects.create(user=username, pwd=password)
    # 从数据库中读取所有数据
    user_list = models.UserInfo.objects.all()

    return render(request, "login.html", {"data":user_list})
# render接收的第三个参数是后台返回给浏览器的数据，一个字典。
# data是字典的键，是你在login.html中自定义的指针名字，对应引用值。
```

重启web服务，刷新浏览器页面，之后和用户交互的数据都能保存到数据库中。
任何时候都可以从数据库中读取数据，展示到页面上。

![](https://www.tielemao.com/wp-content/uploads/2018/06/django-sqlite_login.jpg)

至此，一个要素齐全，主体框架展示清晰的简单django项目完成了。

【end】

参考引用：
http://www.liujiangblog.com/blog/3/

