# CentOS7中部署Showdoc

文：铁乐与猫

[[toc]]

## 前置环境

因为showdoc其实就是可以认为是一个php网站，所以从GitHub上下载整个代码包回来部署在服务器的网站目录上就好了。
但前提是你php和网站环境要先搭建好。
这里实操我使用的是比较熟悉的nginx+PHP

* 系统是CentOS7.2 64位
* nginx已编译安装好
* php环境通过yum安装一下，会比较好解决依赖关系。

```
yum install php php-gd php-fpm php-mcrypt php-mbstring php-mysql php-pdo
```

![1]($res/1.jpg)

## 部署

### 配置文件
安装完后，在你安装的ngix目录下`/nginx/conf/nginx.conf`中`http区块`中加入如下配置:

>include vhosts/showdoc.conf;

当然你也可以不单独为它列一个配置文件而是直接写进`nginx.conf`中,而我这里是将接下来的配置写到`vhosts`目录下的`showdoc.conf`中：

```
server {
            listen       80;
            server_name  showdoc.testytowin.com;       
#自定义的域名，因为我在内部有自己的DNS服务器
            root         /var/www/html;                                         
#这个目录看有没有必要再重新指定，默认打算将showdoc代码包解到这里也行
            index index.php index.html
            error_page  404              /404.html;
            location = /40x.html {
            }
            error_page   500 502 503 504  /50x.html;
            location = /50x.html {
            }
            location ~ \.php$ {
                root           /var/www/html;
                fastcgi_pass   127.0.0.1:9000;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
                include        fastcgi_params;
            }
            location ~ /\.ht {
                deny  all;
            }
        }
```

使用语法检测过也正常。

### 解压安装包

那么接下来就是下载代码包解压放到服务器上了。

然后进入目录`/var/www/html`（不存在则新建），将ShowDoc上传并按照部署手册(http://blog.star7th.com/2016/05/2007.html )安装即可。
我之前是在GitHub下载好了代码包，zip格式的，上传后还在服务器上折腾了一下unzip解压。现html文件夹下可以看到如下图:

![2]($res/2.jpg)

### 添加启动服务

准备网页浏览自己服务器上的showdoc安装前先执行命令将nginx和php解析器启动，php-fpm由于是yum安装的可以再将服务添加到开机启动

```
service nginx start          
#nginx如果是yum安装的才可以这样启动，我是编译安装的，所以我会再进nginx目录手动执行nginx脚本启动
service php-fpm start
chkconfig php-fpm on
chkconfig nginx on            
#如果是yum安装的nginx可以添加到服务开机启动，因为我是编译安装的，我会用脚本去做成开机启动
```
* 启动Nginx
```
/application/nginx/sbin/nginx 
#这是我编译安装nginx的目录
```
* 启动php服务
```
[root@jira sbin]# service php-fpm start
Redirecting to /bin/systemctl start  php-fpm.service
[root@jira sbin]# chkconfig php-fpm on
注意：正在将请求转发到“systemctl enable php-fpm.service”。
Created symlink from /etc/systemd/system/multi-user.target.wants/php-fpm.service to /usr/lib/systemd/system/php-fpm.service.
```
### 设置权限

* 文件夹权限
请确保`/Application/Runtime` 、 `/Public/Uploads` 、 `/Sqlite` 、 `/Sqlite/showdoc.db.php` 有可写权限

* Windows服务器
在php.ini里面把`extension=php_sqlite.dll`和`extension=php_pdo_sqlite.dll`启用以便开启对SQlite的支持；也启用`php_mbstring.dll`；

Linux服务器则不需要此操作。

### 运行安装

在浏览器访问`http://xxxx.com/install/ `（请将网址更改为你服务器域名或ip）。

安装完毕后可使用默认账号showdoc（密码：123456）登录，也可以自行注册账户

我还没做给权限的操作，并且也不是访问install，会像如下报错：

![3]($res/3.jpg)


浏览器重新访问，后面加上`/install` （例如我的是`172.16.0.111/install`或`http://showdoc.testytowin.com/install`
出现选择语言的界面，证明已经可以正常使用安装了。

![4]($res/4.jpg)

下一步提示给权限，进服务器操作chmod,权限给予就不累述了:

![5]($res/5.jpg)

但安装`php-mcrypt`这个倒是有点麻烦，
之前yum 时也敲过安装的，显然是yum源没找着安装不上。

是换源还是直接跑去下载安装好了？

所以后面还是感觉docker容器部署的好处多。这是题外话。

```
yum  install epel-release  //扩展包更新包
yum  update //更新yum源
yum  install libmcrypt libmcrypt-devel mcrypt mhash
```

使用了更新yum源的方法来安装`php-mcrypt`

![6]($res/6.jpg)

我好像搞错了，我现在yum装的是mcrypt并没有装php-mcrypt，难怪还是不成功。
`yum  install  php-mcrypt`

![7]($res/7.jpg)

这个才是真的安装成功！

重来刷新再进行下去（我甚至还重启过了系统）


![8]($res/8.jpg)

sqlite目录这个也要有权限，官网博客说明里有提到了，没仔细留意到，再去赋权，包括它的`/Sqlite/showdoc.db.php`要有写权限。

刷新页面

![9]($res/9.jpg)

安装成功了。

### 界面

感觉不但install这个目录要删除，之前给得过高的权限也得调整，不过现在先进网站首页看看。

![10]($res/10.jpg)

注册了一个用户，然后尝试新建了一个测试项目看

![11]($res/11.jpg)

可以看到它和其它富文本编辑器相比，多了一些API接口模板，有JSON工具等，挺适合开发写这种文档的。

后续的使用以后再发博文。

【end】