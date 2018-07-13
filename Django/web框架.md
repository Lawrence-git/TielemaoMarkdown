# web框架
[toc]

## 什么是web框架
* Web框架（Web framework）是一种开发框架，用来支持动态网站、网络应用和网络服务的开发。
    * 大多数的web框架提供了一套开发和部署网站的方式，也为web行为提供了一套通用的方法。web框架已经实现了很多功能，开发人员使用框架提供的方法并且完成自己的业务逻辑，就能快速开发web应用了。也可以说web框架就是在以上十几行代码基础张扩展出来的，有很多简单方便使用的方法，大大提高了开发的效率。

* web 框架的目的：向程序员隐藏了处理 HTTP 请求和响应相关的基础代码。
    * 至于隐藏多少这取决于不同的框架，Django 和 Flask 走向了两个极端：Django 包括了每种情形，几乎成了它致命的一点；Flask 立足于“微框架”，仅仅实现 web 应用需要的最小功能，其它的不常用的 web 框架任务交由第三方库来完成。

* Python web 框架都以相同的方式工作的：它们接收 HTTP 请求，分派代码，产生 HTML，创建带有内容的 HTTP 响应。
    * 事实上，所有主流的服务器端框架都以这种方式工作的（ JavaScript 框架除外）。但愿了解了这些框架的目的，你能够在不同的框架之间选择适合你应用的框架进行开发。

* 通俗点的说法，web框架就是别人己经设定好的一个web网站模板，你下载回来后，按照它的规则，“填空”和“修改”成你想要的样子。

* 一般的web框架架构图：
![](https://www.tielemao.com/wp-content/uploads/2018/06/web_frame.jpg)

    * 其它基于python的web框架，如tornado、flask、webpy都是在这个范围内进行增删裁剪的。例如tornado用的是自己的异步非阻塞“wsgi”，flask则只提供了最精简和基本的框架。Django则是直接使用了WSGI，并实现了大部分功能。

## wsgiref模块
最简单的Web应用就是先把HTML用文件保存好，用一个现成的HTTP服务器软件，接收用户请求，从文件中读取HTML，返回。

如果要动态生成HTML，就需要自己来实现上述步骤。不过，接受HTTP请求、解析HTTP请求、发送HTTP响应都是苦力活，如果我们自己来写这些底层代码，还没开始写动态HTML呢，就得花个把月去读HTTP规范。

正确的做法是底层代码由专门的服务器软件实现，用Python专注于生成HTML文档。因为我们不希望接触到TCP连接、HTTP原始请求和响应格式，所以，需要一个统一的接口协议来实现这样的服务器软件，让我们专心用Python编写Web业务。这个接口就是WSGI：Web Server Gateway Interface。而wsgiref模块就是python基于wsgi协议开发的服务模块。

```python
from wsgiref.simple_server import make_server


def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<h1>Hello, web!</h1>']


httpd = make_server('', 8080, application)

print('Serving HTTP on port 8080...')
# 开始监听HTTP请求:
httpd.serve_forever()
```

## DIY一个web框架
建立的目录树如下：
![](https://www.tielemao.com/wp-content/uploads/2018/06/DIY_Web.jpg)

models.py # 和数据库有关
```python

import pymysql

# 连接数据库
conn = pymysql.connect(host='127.0.0.1', port=3306, user='root', passwd='654321', db='web')

# 创建游标
cur = conn.cursor()

sql = '''
create table userinfo(
    id INT PRIMARY KEY,
    name VARCHAR(32),
    PASSWORD VARCHAR(32)
)
'''

cur.execute(sql)

# 提交
conn.commit()

# 关闭指针对象
cur.close()

# 关闭连接对象
conn.close()
```

启动文件manage.py

```python
from wsgiref.simple_server import make_server

from app01.views import *
import urls


def routers():
    URLpattern = urls.URLpattern
    return URLpattern


def applications(environ, start_response):
    path = environ.get("PATH_INFO")
    start_response('200 Ok', [('Content-Type', 'text/html'), ('Charset', 'utf8')])
    urlpattern = routers()
    func = None
    for item in urlpattern:
        if path == item[0]:
            func = item[1]
            break
    if func:
        return [func(environ)]
    else:
        return [b'<h1>404!<h1>']


if __name__ == '__main__':
    server = make_server("", 8889, applications)
    print("server is working...")
    server.serve_forever()
```

urls.py 路由控制
```python
from app01.views import *

URLpattern = (
    ('/login/', login),
)
```

views.py  视图（控制）函数

```python
import pymysql

from urllib.parse import parse_qs


def login(request):
    if request.get("REQUEST_METHOD") == "POST":

        try:
            request_body_size = int(request.get('CONENT_LENGTH', 0))
        except(ValueError):
            request_body_size = 0

        request_body = request['wsgi.input'].read(request_body_size)
        data = parse_qs(request_body)

        user = data.get(b'user')[0].decode("utf8")
        pwd = data.get(b'pwd')[0].decode("utf8")

        # 连接数据库
        conn = pymysql.connect(host='127.0.0.1', port=3306, user='root', passwd='654321', db='web')
        # 创建游标
        cur = conn.cursor()
        SQL = "select * from userinfo WHERE NAME ='%s' AND PASSWORD = '%s'" % (user, pwd)
        cur.execute(SQL)

        if cur.fetchone():
            f = open("templates/backend.html", "rb")
            data = f.read()
            data = data.decode("utf8")
            return data.encode("utf8")

        else:
            print("OK456")
            return b"user or pwd is wrong"

    else:
        f = open("templates/login.html", "rb")
        data = f.read()
        f.close()
        return data
```

login.html 登录页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h3>登录页面</h3>
<form action="" method="post">
    用户名<input type="text" name="user">
    密码<input type="password" name="pwd">
    <input type="submit">
</form>

</body>
</html>
```

backend.html 备用的欢迎页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h4>welcome to tielemao_blog!</h4>
</body>
</html>
```

这样子自己DIY的一个简单的web框架就出来了。
下载web_demo这个框架加以修改就可以快速实现一些简单的web功能。

[end]
