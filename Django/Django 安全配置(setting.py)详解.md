# [Django 安全配置(setting.py)详解](https://segmentfault.com/a/1190000003756582)


# Django 安全配置(setting.py)详解

## 1\. 必须配置:

### **0x01\. PASSWORD_HASHER**

这个配置是在使用`Django`自带的密码加密函数的时候会使用的加密算法的列表.默认如下:

```
PASSWORD_HASHERS = (
    'django.contrib.auth.hashers.PBKDF2PasswordHasher',
    'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
    'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',
    'django.contrib.auth.hashers.BCryptPasswordHasher',
    'django.contrib.auth.hashers.SHA1PasswordHasher',
    'django.contrib.auth.hashers.MD5PasswordHasher',
    'django.contrib.auth.hashers.CryptPasswordHasher',
)
```

默认使用第一个条目的加密算法,即`PBKDF2`算法.
所以在使用`make_password`,`check_password`,`is_password_unable`等密码加解密函数的时候,需要添加这个list在`setting.py`文件中,推荐使用默认配置的算法.
**相关链接**:
[https://docs.djangoproject.com/en/1.8/ref/settings/#password-hashers](https://docs.djangoproject.com/en/1.8/ref/settings/#password-hashers)
[https://docs.djangoproject.com/en/1.8/topics/auth/passwords/](https://docs.djangoproject.com/en/1.8/topics/auth/passwords/)

* * *

### **0x02\. ADMINS**

`ADMINS`是一个二元元组,记录开发人员的姓名和email，当DEBUG为False而views发生异常的时候发email通知这些开发人员.类如:

```
(('John', 'john@example.com'), ('Mary', 'mary@example.com'))
```

**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#admins](https://docs.djangoproject.com/en/1.8/ref/settings/#admins)

* * *

### **0x03\. ALLOWED_HOSTS**

`ALLOWED_HOSTS`是为了限定请求中的host值,以防止黑客构造包来发送请求.只有在列表中的host才能访问.强烈建议不要使用`*`通配符去配置,另外当`DEBUG`设置为`False`的时候必须配置这个配置.否则会抛出异常.配置模板如下:

```
ALLOWED_HOSTS = [
    '.example.com',  # Allow domain and subdomains
    '.example.com.',  # Also allow FQDN and subdomains
]
```

**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#allowed-hosts](https://docs.djangoproject.com/en/1.8/ref/settings/#allowed-hosts)

* * *

### **0x04\. DEBUG**

`DEBUG`配置为`True`的时候会暴露出一些出错信息或者配置信息以方便调试.但是在上线的时候应该将其关掉，防止配置信息或者敏感出错信息泄露.

```
    DEBUG = False
```

**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#std](https://docs.djangoproject.com/en/1.8/ref/settings/#std):setting-DEBUG

* * *

### **0x05\. INSTALLED_APPS**

`INSTALLED_APPS`是一个一元数组.里面是应用中要加载的自带或者自己定制的app包路径列表.

```
INSTALLED_APPS = [
    'anthology.apps.GypsyJazzConfig',
    # ...
]
```

**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#installed-apps](https://docs.djangoproject.com/en/1.8/ref/settings/#installed-apps)
[https://docs.djangoproject.com/en/1.8/ref/applications/](https://docs.djangoproject.com/en/1.8/ref/applications/)

* * *

### **0x06\. MANAGERS**

和`ADMINS`类似,并且结构一样,当出现'broken link'的时候给manager发邮件.
**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#std](https://docs.djangoproject.com/en/1.8/ref/settings/#std):setting-MANAGERS

* * *

### **0x07\. MIDDLEWARE_CLASSES**

web应用中需要加载的一些中间件列表.是一个一元数组.里面是django自带的或者定制的中间件包路径,如下：

```
MIDDLEWARE_CLASSES = (
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'django.middleware.security.SecurityMiddleware',
)
```

**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#std](https://docs.djangoproject.com/en/1.8/ref/settings/#std):setting-MIDDLEWARE_CLASSES
[https://docs.djangoproject.com/en/1.8/topics/http/middleware/](https://docs.djangoproject.com/en/1.8/topics/http/middleware/)

* * *

### **0x08\. TEMPLATE_DEBUG**

同样是一个DEBUG开关,若为`True`,DEBUG信息在触发异常之后,会显示在网页上.上线之前必须修改成:

```
TEMPLATE_DEBUG = False
```

**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#std](https://docs.djangoproject.com/en/1.8/ref/settings/#std):setting-TEMPLATE_DEBUG

* * *

* * *

## 2\. 建议配置

* * *

### **0x01\. DEBUG**

```
DEBUG = False
```

防止配置信息和调试信息暴露

* * *

### **0x02\. SESSION_COOKIE_SECURE**

```
SESSION_COOKIE_SECURE = True
```

使得`session cookie`被标记上`secure`标记,从而只能传输在`HTTPS`下
**相关链接:**
[https://docs.djangoproject.com/en/1.8/ref/settings/#session-cookie-secure](https://docs.djangoproject.com/en/1.8/ref/settings/#session-cookie-secure)

* * *

### **0x03\. SESSION_COOKIE_HTTPONLY**

```
SESSION_COOKIE_HTTPONLY = True
```

使得`session cookie`被标记上`http only`标记,从而只能被http协议读取,不能被`Javascript`读取

* * *

### **0x04\. TEMPLATE_DEBUG**

```
TEMPLATE_DEBUG = False
```

防止配置信息和debug信息通过view传出.

* * *

* * *

## 3\. 推荐的中间件:

* * *

### **0x01\. SessionMiddleware**

1.  配置作用:
    在应用中使用session

2.  配置方法:
    在MIDDLEWARE_CLASSES中加入:
    `django.contrib.sessions.middleware.SessionMiddleware`

3.  相关链接:
    [https://docs.djangoproject.com/en/1.8/ref/middleware/](https://docs.djangoproject.com/en/1.8/ref/middleware/)
    [https://docs.djangoproject.com/en/1.8/topics/http/sessions/](https://docs.djangoproject.com/en/1.8/topics/http/sessions/)

* * *

### **0x02\. CsrfViewMiddleware**

1.  配置作用:
    在应用中添加CSRF token用来防范csrf攻击

*   配置方法:

1.  1.在MIDDLEWARE_CLASSES中加入:
    `django.contrib.sessions.middleware.CsrfViewMiddleware`

2.  相关链接:
    [https://docs.djangoproject.com/en/1.8/ref/middleware/](https://docs.djangoproject.com/en/1.8/ref/middleware/)
    [https://docs.djangoproject.com/en/1.8/ref/csrf/](https://docs.djangoproject.com/en/1.8/ref/csrf/)

### **0x03\. clickjacking.XFrameOptionsMiddleware**

1.  配置作用:
    在Http header中添加 `X-Frame-Options` 标志.防范Clickjacking

*   配置方法:

1.  1.在MIDDLEWARE_CLASSES中加入:
    `django.middleware.clickjacking.XFrameOptionsMiddleware`

2.  相关链接:
    [https://docs.djangoproject.com/en/1.8/ref/clickjacking/](https://docs.djangoproject.com/en/1.8/ref/clickjacking/)

* * *

* * *

## 4\. 推荐安装的app:

* * *

### **0x01\. django_bleach**

作用:过滤html字符串,返回合法的已经过滤的安全html字符串.
官方链接:[https://bitbucket.org/ionata/django-bleach](https://bitbucket.org/ionata/django-bleach)
文档:[https://django-bleach.readthedocs.org/en/latest/](https://django-bleach.readthedocs.org/en/latest/)

### **0x02\. xframeoptions**

作用:防范ClickJacking,作用和官方的XFrameOptionsMiddleware相似
官方链接:[https://github.com/paulosman/django-xframeoptions](https://github.com/paulosman/django-xframeoptions)