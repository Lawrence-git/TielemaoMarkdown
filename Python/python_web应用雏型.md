# python_web应用雏型

Web应用程序顾名思义，就是一种可以通过Web访问的应用程序，
Web应用的最大特点是用户只需要有网络和浏览器，不需要再安装其他软件就可顺利通过web访问到程序。

WEB应用程序一般是B/S模式(浏览器端/服务器端)。
Web应用程序首先是“应用程序”，和用标准的程序语言，如java，python等编写出来的程序没有什么本质上的不同。
而在网络编程的意义下，浏览器是一个socket客户端，服务器是一个socket服务端。

例：一个简单的web应用程序雏型
```python
import socket

sock = socket.socket()
sock.bind(('127.0.0.1', 8800))
sock.listen(5)

while 1:
    conn, addr = sock.accept()
    conn.recv(1024)
    conn.send(b"HTTP/1.1 200 OK\r\n status: 200\r\n Content-Type:text/html\r\n\r\n")
    conn.send(b'<h1>Hello world<h1>')
    conn.close()
```
运行代码后，做为一个web服务端，开放本地8800端口。
通过浏览器访问的效果如图：

![](https://www.tielemao.com/wp-content/uploads/2018/06/hello_world_web.jpg)

这里要注意的是服务端发送客户端（浏览器）的byte字节信息中要包含http协议的信息，浏览器才可以正确解析出响应的信息。
如果将`HTTP/1.1 200 OK`这些信息给去掉，浏览器就会返回非法响应的报错信息，如图：

![](https://www.tielemao.com/wp-content/uploads/2018/06/err_invalid.jpg)

关于http协议在下一篇博文中再详谈。
在这篇文章中，我们理解web应用本质上也是socket服务端和客户端的交互，当然后面我们写代码不会是如此简陋麻烦，
会借助一些模块或 web框架去完成解析http协议的事情。

【end】
2018-6-21