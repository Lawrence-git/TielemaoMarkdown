# Http协议浅析
[toc]

## http协议简介

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,
是用于万维网**服务器与本地浏览器**之间传输超文本的传送协议。

HTTP是一个属于应用层的面向对象的协议，HTTP协议工作于客户端-服务端架构。
浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。
Web服务器根据接收到的请求后，向客户端发送响应信息。

## http协议特性

* (1) 基于TCP/IP
    * http 协议是基于TCP/IP协议之上的应用层协议。
* (2) 基于请求-响应模式
    * HTTP协议规定,请求从客户端发出,最后服务器端响应该请求并返回。
    * 换句话说,肯定是先从客户端开始建立通信的,服务器端在没有接收到请求之前不会发送响应。
* (3) 无状态保存
    * HTTP是一种不保存状态,即无状态(stateless)协议。HTTP协议自身不对请求和响应之间的通信状态进行保存。也就是说在HTTP这个级别,协议对于发送过的请求或响应都不做持久化处理。
    * 使用HTTP协议,每当有新的请求发送时,就会有对应的新响应产生。协议本身并不保留之前一切的请求或响应报文的信息。这是为了更快地处理大量事务,确保协议的可伸缩性,而特意把HTTP协议设计成如此简单的。
    * 可是,随着Web的不断发展,因无状态而导致业务处理变得棘手的情况增多了。比如,用户登录到一家购物网站,即使他跳转到该站的其他页面后,也需要能继续保持登录状态。针对这个实例,网站为了能够掌握是谁送出的请求,需要保存用户的状态。HTTP/1.1虽然是无状态协议,但为了实现期望的保持状态功能, 于是引入了Cookie技术。有了Cookie再用HTTP协议通信,就可以管理状态了。
* (4) 无连接
    * 无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

## http请求协议与响应协议

用于HTTP协议交互的信被为HTTP报文。
请求端(客户端)的HTTP报文叫做请求报文,响应端(服务器端)的叫做响应报文。
HTTP报文本身是由多行数据构成的字文本。

![](https://www.tielemao.com/wp-content/uploads/2018/06/message.png)

以下面的客户端发送给某http服务器端的请求报文为例：

```http
GET /index.htm HTTP/1.1
Host: hackr.jp
```

起始行开头的GET表示请求访问服务器的类型，称为方法（method）。
随后的字符串/index.htm指明了请求访问的资源对象，也叫做请求URI（request-URI）。
最后的HTTP/1.1，即HTTP的版本号，用来提示客户端使用的HTTP协议功能。

### 请求协议

请求报文是由请求方法、请求URI、协议版本、可选的请求首部字段和内容实体构成的。
请求报文的构成如下图：
![](https://www.tielemao.com/wp-content/uploads/2018/06/request.png)


### 响应协议

响应报文的构成如下图：
![](https://www.tielemao.com/wp-content/uploads/2018/06/response.png)

在起始行开头的HTTP/1.1表示服务器对应的HTTP版本。
紧挨着的200 OK表示请求的处理结果的状态码（status code）和原因短语（reason-phrase)。
下一行显示了创建响应的日期时间，是首部字段（headr field）内的一个属性。

接着以一行空行分隔，之后的内容称为资源实体的主体（entity body）。
响应报文基本上由协议版本、状态码（表示请求成功或失败的数字代码）、
用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成。

### 响应状态码

状态码的职责是当客户端向服务器端发送请求时, 返回请求结果。
借助状态码,用户可以知道服务器端是正常处理了请求,还是出现了错误。
状态码如200 OK,以3位数字和原因构成。
响应分别有以下5种。

![](https://www.tielemao.com/wp-content/uploads/2018/06/error.png)

### 请求URI定位资源

HTTP协议使用URI定位互联网上的资源。
正是因为URI的特定功能，在互联网上任意位置的资源都能访问到。

## HTTP方法

客户端告知服务器端意图的方法，主要如下：

### GET：获取资源

GET方法用来请求访问己被URI识别的资源。指定的资源经服务器端解析后返回响应内容。
例如，请求的资源是文本，就保持原样返回；
如果是像CGI（Common Gateway Interface，通用网关接口）那样的程序，则返回经过执行后的输出结果。

### POST：传输实体主体

POST方法用来传输实体的主体。
虽然用GET方法也可以传输实体的主体，但一般不用GET方法进行传输，而是用POST方法。
虽说POST的功能与GET很相似，但POST的主要目的并不是获取响应的主体内容。

### PUT：传输文件

PUT方法用来传输文件。
就像FTP协议的文件上传一样，要求在请求报文的主体中包含文件内容，
然后保存到请求URI指定的位置。

但是，鉴于HTTP/1.1的PUT方法自身不带验证机制，任何人都可以上传文件，
存在安全性问题，因此一般的web网站不使用该方法。

### HEAD：获得报文首部

HEAD方法和GET方法一样，只是不返回报文主体部分。
用于确认URI的有效性及资源更新的日期时间等。

### DELETE：删除文件

DELETE方法用来删除文件，是与PUT相反的方法。
DELETE方法按请求URI删除指定的资源。

同样，HTTP/1.1的DELETE方法本身和PUT方法一样不带验证机制，
所以一般的web网站也不使用DELETE方法。

### OPTIONS：询问支持的方法

OPTIONS方法用来查询针对请求URI指定的资源支持的方法。

### TRACE：追踪路径

TRACE方法是让web服务器端将之前的请求通信环回给客户端的方法。
发送请求时，在Max-Forwards首部字段中填入数值，每经过一个服务器端就将该数字减1，
当数值刚好减到0时，就停止继续传输，最后接收到请求的服务器端则返回状态码200 OK的响应。

客户端通过TRACE方法可能查询发送出去的请求是怎样被加工修改/篡改的。
这是因为，请求想要连接到源目标服务器可能会通过代理中转，
TRACE方法就是用来确认连接过程中发生的一系列操作。

但是，TRACE方法本来就不怎么常用，
再加上它容易引发XST（Cross-Site Tracing，跨站追踪）攻击，通常就更不会用到了。

### CONNECT：要求用隧道协议连接代理

CONNECT方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信。
主要使用SSL和TLS协议把通信内容加密后经网络隧道传输。

CONNECT方法的格式如下所示。
`CONNECT 代理服务器名：端口号 HTTP版本`

以上方法中主要常见和使用的是get和post方法。

**请求方式: get与post请求的区别**

* GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditBook?name=test1&id=123456. POST方法是把提交的数据放在HTTP包的请求体中。
* GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制。
* GET与POST请求在服务端获取请求数据方式不同。

## 示例，python创建web应用：

```python
import socket

sock=socket.socket()
sock.bind(("127.0.0.1",8800))
sock.listen(5)

while 1:
    print("server waiting.....")
    conn,addr=sock.accept()
    data=conn.recv(1024)
    print("data",data)

    # 读取html文件
    with open("login.html","rb") as f:
        data=f.read()

    conn.send((b"HTTP/1.1 200 OK\r\n\r\n%s"%data))
    conn.close()
```

login.html中的代码如下：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<form action="" method="post">
    用户名 <input type="text" name="user">
    密码 <input type="password" name="pwd">
    <input type="submit">
</form>

</body>
</html>
```

参考引用：
《图解HTTP》

【end】