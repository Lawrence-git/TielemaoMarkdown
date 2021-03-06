# 基于nginx和uWSGI在Ubuntu上部署Django

Posted by [Gevin](https://blog.igevin.info/users/gevin/) on 2015/11/18, 9:11 am Category: [python](https://blog.igevin.info/?category=python) Tags: [django](https://blog.igevin.info/?tag=django)

> 本文内容基于 [uWSGI](http://uwsgi-docs.readthedocs.org/en/latest/tutorials/Django_and_nginx.html)文档整理，有问题欢迎与我讨论或者直接查看原文档

# 1\. nginx

### 安装

```
sudo apt-get install nginx

```

### 启动、停止和重启

```
sudo /etc/init.d/nginx start
sudo /etc/init.d/nginx stop
sudo /etc/init.d/nginx restart

```

或者

```
sudo service nginx start
sudo service nginx stop
sudo service nginx restart

```

# 2\. uWSGI安装

用python的pip安装最简单：

```
sudo apt-get install python-dev #不安装这个，下面的安装可能会失败
sudo pip install uwsgi

```

# 3\. 基于uWSGI和nginx部署Django

## 1.原理

基于nginx和uwsgi部署django后，从客户端发起请求到服务器响应请求，会经过一下几个环节：

```
the web client <-> the web server(nginx) <-> the socket <-> uwsgi <-> Django

```

## 2.基本测试

### 测试uWSGI是否正常

在django项目的根目录下创建test.py文件，添加源码如下：

```
# test.py
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return ["Hello World"] # python2
    #return [b"Hello World"] # python3

```

然后，Run uWSGI:

```
uwsgi --http :8000 --wsgi-file test.py

```

参数含义:

*   `http :8000`: 使用http协议，8000端口
*   `wsgi-file` test.py: 加载指定文件 test.py

打开下面url，浏览器上应该显示`hello world`

```
http://example.com:8000

```

如果显示正确，说明下面3个环节是通畅的：

```
the web client <-> uWSGI <-> Python

```

### 测试Django项目是否正常

首先确保project本身是正常的：

```
python manage.py runserver 0.0.0.0:8000

```

如果没问题，使用uWSGI把project拉起来：

```
uwsgi --http :8000 --module mysite.wsgi

```

*   `module mysite.wsgi`: 加载wsgi module

如果project能够正常被拉起，说明以下环节是通的：

```
the web client <-> uWSGI <-> Django

```

## 3.配置nginx

安装nginx完成后，如果能正常打开`http://hostname`，说明下面环节是通畅的：

```
the web client <-> the web server

```

### 增加nginx配置

*   将`uwsgi_params`文件拷贝到项目文件夹下。`uwsgi_params`文件在`/etc/nginx/`目录下，也可以从[这个页面](https://github.com/nginx/nginx/blob/master/conf/uwsgi_params)下载
*   在项目文件夹下创建文件`mysite_nginx.conf`,填入并修改下面内容：

```
# mysite_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    # server unix:///path/to/your/mysite/mysite.sock; # for a file socket
    server 127.0.0.1:8001; # for a web port socket (we'll use this first)
}
# configuration of the server
server {
    # the port your site will be served on
    listen      8000;
    # the domain name it will serve for
    server_name .example.com; # substitute your machine's IP address or FQDN
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    # Django media
    location /media  {
        alias /path/to/your/mysite/media;  # your Django project's media files - amend as required
    }

    location /static {
        alias /path/to/your/mysite/static; # your Django project's static files - amend as required
    }

    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     /path/to/your/mysite/uwsgi_params; # the uwsgi_params file you installed
    }
}

```

这个configuration文件告诉nginx从文件系统中拉起media和static文件作为服务，同时相应django的request

在`/etc/nginx/sites-enabled`目录下创建本文件的连接，使nginx能够使用它：

```
sudo ln -s ~/path/to/your/mysite/mysite_nginx.conf /etc/nginx/sites-enabled/

```

### 部署static文件

在django的setting文件中，添加下面一行内容：

```
STATIC_ROOT = os.path.join(BASE_DIR, "static/")

```

然后运行：

```
python manage.py collectstatic

```

### 测试nginx

首先重启nginx服务：

```
sudo /etc/init.d/nginx restart

```

然后检查media文件是否已经正常拉起： 在目录`/path/to/your/project/project/media directory`下添加文件`meida.png`，然后访问http://example.com:8000/media/media.png ，成功后进行下一步测试。

## 4.nginx and uWSGI and test.py

执行下面代码测试能否让nginx在页面上显示`hello, world`

```
uwsgi --socket :8001 --wsgi-file test.py

```

访问http://example.com:8000 ,如果显示`hello world`，则下面环节是否通畅:

```
the web client <-> the web server <-> the socket <-> uWSGI <-> Python

```

### 用UNIX socket取代TCP port

对`mysite_nginx.conf`做如下修改：

```
server unix:///path/to/your/mysite/mysite.sock; # for a file socket
# server 127.0.0.1:8001; # for a web port socket (we'll use this first)

```

重启nginx，并在此运行`uWSGI`

```
uwsgi --socket mysite.sock --wsgi-file test.py

```

打开 http://example.com:8000/ ，看看是否成功

#### 如果没有成功:

检查 nginx error log(/var/log/nginx/error.log)。如果错误如下：

```
connect() to unix:///path/to/your/mysite/mysite.sock failed (13: Permission
denied)

```

添加socket权限再次运行：

```
uwsgi --socket mysite.sock --wsgi-file test.py --chmod-socket=666 # (very permissive)

```

or

```
uwsgi --socket mysite.sock --wsgi-file test.py --chmod-socket=664 # (more sensible)

```

## 5.Running the Django application with uswgi and nginx

如果上面一切都显示正常，则下面命令可以拉起django application

```
uwsgi --socket mysite.sock --module mysite.wsgi --chmod-socket=664

```

### Configuring uWSGI to run with a .ini file

每次都运行上面命令拉起django application实在麻烦，使用.ini文件能简化工作，方法如下：

在application目录下创建文件`mysite_uwsgi.ini`，填入并修改下面内容：

```
# mysite_uwsgi.ini file
[uwsgi]

# Django-related settings
# the base directory (full path)
chdir           = /path/to/your/project
# Django's wsgi file
module          = project.wsgi
# the virtualenv (full path)
home            = /path/to/virtualenv

# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /path/to/your/project/mysite.sock
# ... with appropriate permissions - may be needed
# chmod-socket    = 664
# clear environment on exit
vacuum          = true

```

现在，只要执行以下命令，就能够拉起django application：

```
uwsgi --ini mysite_uwsgi.ini # the --ini option is used to specify a file

```

### Make uWSGI startup when the system boots

编辑文件`/etc/rc.local`, 添加下面内容到这行代码之前`exit 0`:

```
/usr/local/bin/uwsgi --socket /path/to/mysite.sock --module /path/to/mysite.wsgi --chmod-socket=666

```

### uWSGI的更多配置

```
env = DJANGO_SETTINGS_MODULE=mysite.settings # set an environment variable
pidfile = /tmp/project-master.pid # create a pidfile
harakiri = 20 # respawn processes taking more than 20 seconds
limit-as = 128 # limit the project to 128 MB
max-requests = 5000 # respawn processes after serving 5000 requests
daemonize = /var/log/uwsgi/yourproject.log # background the process & log

```