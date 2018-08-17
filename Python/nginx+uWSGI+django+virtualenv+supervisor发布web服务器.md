# nginx+uWSGI+django+vue+supervisor部署web服务器
@toc

## 环境说明
* 进行本文操作前需己搭建好的环境
  * linux系统，我用的是openSUSE
  * python3.5.6
  * virtualenv 16.0
  * pip 18.0
  * nginx
* 后面进行安装的环境
  * django 1.11
  * uwsgi-2.0.17.1
  
## 搭建项目
* 建立一个虚拟环境
  * 建议个人学习和测试的话，直接建在家用户目录下，以避免权限引起的各种拒绝问题
  * 或者正式点，系统再建一个dev用户，在专门的工作目录下给好dev用户的权限去运行。
  * 虚拟环境的目录名建议就带个env点明是虚拟环境，如果和后面的项目取相同的名字你会看到三重同名的串在一起，有点不太好认。
`virtualenv -p python3 py3env`

* 启动虚拟环境
`source py3env/bin/activate`

* 安装django1.11，之所以装这个版本是学习所需要，后面自己的项目最好与时俱进。
`pip install django==1.11`

* 直接使用django-admin命令创建一个django项目
  * 建议不要放在py3env目录下，而是在专门的工作目录下建立
`django-admin startproject luffy`

如下，己经初步搭建好简单的项目：
```bash
cd luffy

(py3env) operation@opensuse-wordpress:~/work/luffy> tree -L 2
.
├── luffy
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

* 创建新应用，例如app01
`python3 manage.py startapp app01`
结构如下
```
(py3env) operation@opensuse-wordpress:~/work/luffy> tree -L 2
.
├── app01
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── luffy
│   ├── __init__.py
│   ├── __pycache__
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

## Django部署

### 编辑`luffy/luffy/settings.py`
```python
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = ['*', ]

# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app01',
]
```
如上，主要是：
* 将DEBUG由True改成False（虽然只是在学习和测试搭建的，也要顾及好安全）
* 在ALLOWED_HOSTS 默认的空列表中填入你自己打算使用的域名，我这里测试的时候填的是`*`，真正上线部署的时候不建议填成通配符的`*`，而是要填允许访问的主机域名。一般django用于做后端服务器，而前端会有一个域名。详情可见django官方文档。
* INSTALLED_APPS 列表下增加`appo1`，表示将app01应用给安装注册上。

### 编辑`luffy/app01/views.py`
```python
from django.shortcuts import render,HttpResponse

# Create your views here.

def index(request):
    return HttpResponse('Hello Hero')
```
由于只是简单演示，所以上面只是让用户访问index页面后，得到一个显示`Hello Hero`的页面。

### 编辑`luffy/luffy/urls.py`
```python
from django.conf.urls import url
from django.contrib import admin
from app01 import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/', views.index),
]
```

### 运行并测试

首先是运行，由于是单独对django进行运行测试，所以只是在进入项目根目录后命令行敲以下命令：

`python manage.py runserver 0.0.0.0:8083`
其中`0.0.0.0`表示捆绑监听服务器上的所有网卡IP地址，当然也就包括了127.0.0.1和外网的公网ip了。

最初我测试的是ALLOWED_HOSTS填的允许的HOSTS是自己的域名`.tielemao.com`，结果直接在浏览器上敲IP加8083端口接index当然是报错：
![400err]($resource/400err.jpg)
`Bad Request（400）`而由于我将DEBUG值设为了False，所以也不会将调试信息暴露出来。

然后我将`.tielemao.com`改为了通配符`*`(不安全，表示允许所有主机头）。
再访问`http://39.x.x.x:8083/index/`的时候，就能正常访问到了：
![index-hello]($resource/index-hello.jpg)

* 注意，我使用的是8083端口并且在防火墙和云控制平台上的安全组处己设置了开放该端口。

django能运行无误后，`ctrl+c`将它停止，再接下来做下面的操作。

## uWSGI部署
同样为了环境的隔离和纯净，这次我也选择在同样的虚拟环境下安装：
`pip3 install uwsgi`
安装完成后可`uwsgi --version`查看版本，
`uwsgi --python-version`还可以间接查看到python的版本。

* 编写一个用于简单测试uwsgi的python脚本
`vim test-uwsgi.py`，内容如下：
```python
def application(env,start_response):
    start_response('200 OK',[('Content-Type','text/html')])
    return [b"Hello Hero, all for one "]
```

### 测试运行uWSGI
以下命令表示运行uwsgi服务，同样是在8083端口上开放web访问。
* 注意`--http` 后是一个空格再接`:`端口号。
`uwsgi --http :8083 --wsgi-file test-uwsgi.py`

访问web服务器8083端口，正常显示`test-uwsgi.py`脚本中返回显示的文本。
![uwsgi-web]($resource/uwsgi-web.jpg)

因为我们现在要做的是基于nginx和uwsgi部署django，从客户端发起请求到服务器响应请求，会经过一下几个环节：

```
the web client <-> the web server(nginx) <-> the socket <-> uwsgi <-> Django
```

而单独测试uWSGI运行，访问显示正常的话，说明下面3个环节是通畅的：

```
the web client <-> uWSGI <-> Python
```



