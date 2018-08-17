# （总结）Web性能压力测试工具之WebBench详解

https://blog.csdn.net/sscsgss/article/details/47679691

**PS：在运维工作中，压力测试是一项很重要的工作。比如在一个网站上线之前，能承受多大访问量、在大访问量情况下性能怎样，这些数据指标好坏将会直接影响用户体验。但是，在压力测试中存在一个共性，那就是压力测试的结果与实际负载结果不会完全相同，就算压力测试工作做的再好，也不能保证100%和线上性能指标相同。面对这些问题，我们只能尽量去想方设法去模拟。所以，压力测试非常有必要，有了这些数据，我们就能对自己做维护的平台做到心中有数。**

[Webbench](http://www.ha97.com/tag/webbench "Webbench")是知名的网站压力测试工具，它是由Lionbridge公司（http://www.lionbridge.com）开发。

Webbench能测试处在相同硬件上，不同服务的性能以及不同硬件上同一个服务的运行状况。webbench的标准测试可以向我们展示服务器的两项内容：**每秒钟相应请求数和每秒钟传输数据量。**webbench不但能具有便准静态页面的测试能力，还能对动态页面（ASP,PHP,[JAVA](http://www.ha97.com/category/%E7%BC%96%E7%A8%8B%E5%BC%80%E5%8F%91/java "JAVA"),CGI）进 行测试的能力。还有就是他支持对含有SSL的安全网站例如电子商务网站进行静态或动态的性能测试。
**Webbench最多可以模拟3万个并发连接去测试网站的负载能力。**

官方主页：http://home.tiscali.cz/~cz210552/webbench.[html](http://www.ha97.com/tag/html "html")

官方介绍：
 `Web Bench is very simple tool for benchmarking WWW or [proxy](http://www.ha97.com/tag/proxy "proxy") servers. Uses fork() for simulating multiple clients and can use HTTP/0.9-HTTP/1.1 requests. This benchmark is not very realistic, but it can test if your HTTPD can realy handle that many clients at once (try to run some CGIs) without taking your machine down. Displays pages/min and bytes/sec. Can be used in more aggressive mode with -f switch.` 

1、WebBench安装：
 `wget http://www.ha97.com/[code](http://www.ha97.com/tag/code "code")/webbench-1.5.tar.gz
tar zxvf webbench-1.5.tar.gz
cd webbench-1.5
make
[make install](http://www.ha97.com/tag/make-install "make install")`

2、WebBench使用：
`webbench -c 1000 -t 60 http://192.168.80.157/phpinfo.php`
webbench -c 并发数 -t 运行测试时间 URL

[Apache](http://www.ha97.com/category/web-server/apache "Apache")测试实例结果：
当并发300时，
`root [ ~ ]# webbench -c 300 -t 60 http://192.168.80.157/phpinfo.php
Webbench - Simple Web Benchmark 1.5
Copyright (c) Radim Kolar 1997-2004, GPL Open [Source](http://www.ha97.com/tag/source "Source") Software.`

Benchmarking: GET http://192.168.80.157/phpinfo.php
300 clients, running 60 sec.

`Speed=24525 pages/min, 20794612 bytes/sec.
Requests: 24525 susceed, 0 failed.` 
每秒钟响应请求数：24525 pages/min，每秒钟传输数据量20794612 bytes/sec.

当并发1000时，已经显示有87个连接failed了，说明超负荷了。
 `root [ ~ ]# webbench -c 1000 -t 60 http://192.168.80.157/phpinfo.php
Webbench - Simple Web Benchmark 1.5
Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.`

Benchmarking: GET http://192.168.80.157/phpinfo.php
1000 clients, running 60 sec.

`Speed=24920 pages/min, 21037312 bytes/sec.
Requests: 24833 susceed, 87 failed.` 
并发1000[运行](http://www.ha97.com/tag/%E8%BF%90%E8%A1%8C "运行")60秒后产生的TCP连接数12000多个：
[![](http://www.ha97.com/wp-content/uploads/2012/05/tcp.jpg "tcp")](http://www.ha97.com/wp-content/uploads/2012/05/tcp.jpg)

**总结：
1、压力测试工作应该放到产品上线之前，而不是上线以后；
2、测试时并发应当由小逐渐加大，比如并发100时观察一下网站负载是多少、打开页面是否流畅，并发200时又是多少、网站打开缓慢时并发是多少、网站打不开时并发又是多少；
3、更详细的进行某个页面测试，如电商网站可以着重测试购物车、推广页面等，因为这些页面占整个网站访问量比重较大。**