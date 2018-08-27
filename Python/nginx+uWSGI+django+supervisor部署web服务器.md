# nginx+uWSGI+django+supervisor部署web服务器
@toc

## 环境说明
* 进行本文操作前需己搭建好的环境
  * linux系统，我用的是openSUSE
    * 使用了operation用户的家目录做为测试环境
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
ctrl+c中止程序，再来进行以下的测试。

### 使用uWSGI运行django项目
在虚拟环境下，进入到django根目录下后敲以下命令：
```python
 uwsgi --http :8000 --module luffy.wsgi
```
效果和直接敲以下命令
```python
python manage.py runserver 0.0.0.0:8000
```
是一样的。

* 注：`--module luffy.wsgi`为加载指定的wsgi模块，你项目起的是什么名字，一般就是`项目名.wsgi`。

这样测试下来，可证明web客户端通过uWSGI到Django是通畅的：
```
the web client <-> uWSGI <-> Django
```
ctrl+c中止程序，再来进行以下的测试

### uWSGi热加载Djangoa项目

在启动命令后面加上参数：
```python
uwsgi --http :8083 --module luffy.wsgi --py-autoreload=1
```
同样，这个时候访问服务器8083端口，也就是访问了django项目（luffy）。
![uwsgi-py-autoreload]($resource/uwsgi-py-autoreload.jpg)

而且还多了一个参数：
`--py-autoreload` 监控python模块以触发重载(仅在开发中使用)

```
# 发布命令
command= /home/operation/work/py3env/bin/uwsgi --uwsgi 0.0.0.0:8000 --chdir /home/operation/work/luffy --module luffy.wsgi
# 此时修改django代码，uWSGI会自动加载django程序，页面生效
```
ctrl+c中止程序，再进行下面环节：

## 部署nginx

### nginx配置uwsgi和django

* 将`uwsgi_params`文件拷贝到项目文件夹下。
* `uwsgi_params`文件一般在`/etc/nginx/`目录下，也可以从[nginx的github页面](https://github.com/nginx/nginx/blob/master/conf/uwsgi_params)下载。

`uwsgi_params`配置文件如下：
```
uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;
```

* 拷贝到项目下：
`sudo cp /etc/nginx/uwsgi_params /home/operation/work/luffy/`

* 属主属组等权限重新修改
```
sudo chown operation:operation uwsgi_params
sudo chmod 664 /home/operation/work/luffy/uwsgi_params
```

*   在项目文件夹下创建文件`luffy_nginx.conf`,填入并修改下面内容：

```nginx
# luffy_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    # server unix:///path/to/your/mysite/mysite.sock; 
    # for a file socket
    server 127.0.0.1:8083; 
    # for a web port socket (we'll use this first)
}

# configuration of the server
server {
    # the port your site will be served on
    listen      8000;
    # the domain name it will serve for
    server_name luffy.tielemao.com; 
    # substitute your machine's IP address or FQDN
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    # Django media
    location /media  {
        alias /home/operation/work/luffy/media;  
        # your Django project's media files - amend as required
    }

    location /static {
        alias /home/operation/work/luffy/static; 
        # your Django project's static files - amend as required
    }

    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     /home/operation/work/luffy/uwsgi_params; 
        # the uwsgi_params file you installed
    }
}

```

这个配置文件告诉nginx从文件系统中拉起media和static文件作为服务，
同时响应django的request。

为此，我也在相应的项目目录下先建立起media和static文件夹。

为方便nginx使用，我在`/etc/nginx/vhosts.d`下创建了一个`luffy_nginx.conf`的软链接。当然，也有人是直接用默认的`sites-enabled`目录，自己看着来就行了。

```
sudo ln -s /home/operation/work/luffy/luffy_nginx.conf luffy.conf
```

### django部署static文件

* 在项目文件夹下，建立好静态文件目录：
`mkdir static`

* django的setting文件中，添加一行：
`STATIC_ROOT = os.path.join(BASE_DIR, “static/”)`

* 运行
`python manage.py collectstatic`

* 在项目文件夹下，建立一个media媒体文件目录（用于存放图片音乐等文件做测试）
`mkdir media`

在media目录中，我传入了一个图片文件`princekin_fox.jpg`用于测试nginx。

### 重新加载nginx进行测试

先进行检测，看之前的配置文件有无错误。
`sudo /usr/sbin/nginx -t`

重新加载nginx让软链接的`luffy_nginx.conf`生效。
`sudo /usr/sbin/nginx -s reload`

访问`http://luffy.tielemao.com:8000/media/princekin_fox.jpg`看能否正常访问到图片资源：
![django-media]($resource/django-media.jpg)

注意在做这一步之前，我是己经配置好域名和传送了一个用于测试的图片到服务器上。

我己经能成功访问到资源了，所以接下来再做下一步测试。

###  测试nginx 应用 uWSGI 和 test.py

还记得之前写好的一个测试的`test-uwsgi.py`文件吗？
我们就用配置好的nginx来访问uwsgi启动的`test-uwsgi.py`好了。
首先，启动uwsgi，并且端口是nginx配置中的负载均衡池8083端口：
`uwsgi --socket :8083 --wsgi-file test-uwsgi.py `

访问http://luffy.tielemao.com:8000/，实际上访问的就是uwsgi的8083端口。
![uwsgi-nginx-test]($resource/uwsgi-nginx-test.jpg)

如上图，能正常访问。表明下面的环节通畅：
```
the web client <-> the web server <-> the socket <-> uWSGI <-> Python
```
测试成功后中止程序，以便进行下面环节。

###  用UNIX socket取代TCP port

修改`luffy_nginx.conf`：
```
# luffy_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    server unix:///home/operation/work/luffy/luffy.sock;
    # for a file socket
    #  server 127.0.0.1:8083; 
    # for a web port socket (we'll use this first)
}

```
上面其实是将原来的`server 127.0.0.1:8083`加以注释，而原来加了`#`注释的`server unix:///home/operation/work/luffy/luffy.sock`则取消了注释，也就是不是直接指定端口，而是指定了一个sock文件。

* 要注意的是，这个luffy.sock并不是自己提前写好的什么配置文件，而是由程序自动生成的。如图：
![uwsgi-sock]($resource/uwsgi-sock.jpg)

重新加载nginx，然后在项目下运行uWSGI
```python
uwsgi --socket /home/operation/work/luffy/luffy.sock --wsgi-file test-uwsgi.py 
```

访问http://luffy.tielemao.com:8000/ 报502网关错误。
检查nginx的错误日志`error.log`，一般位置会在`/var/log/nginx/error.log`，建议使用tail察看文件尾部的后10行。
会发现类似以下报权限受阻的错误：
```bash
2018/08/27 20:17:44 [crit] 25771#25771: *1847865 connect() to unix:///home/operation/work/luffy/luffy.sock failed (13: Permission denied) while connecting to upstream, client: 223.72.74.11, server: luffy.tielemao.com, request: "GET / HTTP/1.1", upstream: "uwsgi://unix:///home/operation/work/luffy/luffy.sock:", host: "luffy.tielemao.com:8000"
```

那么就中止uwsgi进程，添加上socket权限后再次运行：
```
uwsgi --socket /home/operation/work/luffy/luffy.sock --wsgi-file test-uwsgi.py --chmod-socket=664
```
还是报错的话，就需将operation用户添加到`nginx组`中了，当然某些系统中nginx组也可能是`www-data`。

将operation用户添加到nginx组中，记得要加上`-a`参数，不然operation将会离开原来的operation组。
```
sudo usermod -a -G nginx operation
```
然后将项目目录也归属到nginx组中
```
sudo chown operation:nginx -R luffy
```
这样就能保证nginx能对`luffy.sock`有足够的权限了。
果然，设置好权限后，就能正常访问了。

每次都运行上面命令来拉起django项目实在不方便，我们可以将这些配置写在`.ini`文件中，能大大简化工作。

停掉uwsgi服务后，见下一环节。

## 使用uwsgi配置文件运行django项目

uwsgi支持ini，xml等多种配置方式，以ini为例，在`/home/operation/work/conf`目录下新建`uwsgi_luffy.ini`，示例配置如下：

```
# luffy_uwsgi.ini file
[uwsgi]

# Django-related settings
# the base directory (full path)
# 项目目录
chdir = /home/operation/work/luffy

# Django's wsgi file
# 导入django项目的wsgi模块
module = luffy.wsgi

# the virtualenv (full path)
# 配置虚拟环境
home = /home/operation/work/py3env

# process-related settings
# master
master = true 

# maximum number of worker processes
# 开启多少个进程数
processes = 4

# the socket (use the full path to be safe)
# socket = 0.0.0.0:8000
# 上面的socket建议配置成一个luffy.socket文件后使用nginx来连接uWSGI运行,不然容易报socket的请求头错误和权限错误等。
socket = /home/operation/work/luffy/luffy.socket

# ... with appropriate permissions - may be needed
# 配置生成的sock文件的权限
chmod-socket = 664 

# clear environment on exit
＃　退出时清空环境，其实就是将自动生成的luffy.sock文件给干掉。个人环境比较稳定的话我建议还是不用设置成true，注释掉或许还比较好。
vacuum = true
```

uwsgi指定配置文件启动django项目，建议使用nginx用户执行：
`uwsgi --ini /home/operation/work/conf/uwsgi_luffy.ini`

* 注：以上启动后如果你是在配置文件中直接指定的`socket = 0.0.0.0:8000`可能会产生如下问题:浏览器访问服务器8000端口加上url后，浏览器会报连接出错，而服务器运行后台也会看到如下错误信息：
`invalid request block size: 21573 (max 4096)...skip`

之前我们直接使用http协议的方式就不会出现块请求大小超出
```python
 uwsgi --http :8000 --module luffy.wsgi
```
究其原因，使用配置文件启动是以socket方式提供通信端口，默认的协议是tcp，和http不同。socket请求头默认大小是4096，所以请求头超出默认大小后就会出现错误。当然后面我们可以通过和nginx合作的方式解决。

而如果只是想测试，那么只要在上面的命令后面再指定块请求大小`-b 24576`之类的便可以解决。

我们中止uwsgi后重新指定块大小，运行命令：
`uwsgi -b 24576 --ini /home/operation/work/conf/uwsgi_luffy.ini `
可以解决请求头错误。

不过ini配置文件主要是用于和nginx配合，这也是为什么前面讲述完nginx的部署后再回过头来讲uwsgi的ini配置文件。

* 这是因为将uWSGI中使用的相同选项放入一个配置文件中，然后要求uWSGI使用该文件运行。这会使得管理配置变得更加容易。

* 要注意的是，因为要配合nginx，所以生成的`项目名.sock`文件，nginx需要能有权限读写。

如下图：
![uwsgi-sock-nginx]($resource/uwsgi-sock-nginx.jpg)
由于我前面执行uwsgi命令时使用的是operation用户，
这样子自动生成的`luffy.sock`文件属组并不是nginx的，所以前面敲命令用uwsgi执行ini配置文件时估计最好也要使用nginx用户执行。
我是修改完`luffy.sock`的属组为nginx后就能正常访问到django项目了，不然会被nginx报502错误。

## 安装uWSGI到真实环境中

到目前为止，我们都是在虚拟环境下工作的，后期我们会将uwsgi装在实际环境中，且将uwsgi加入到nginx组（或`www-data`)中，就可避免现在所遇到的权限等问题。


