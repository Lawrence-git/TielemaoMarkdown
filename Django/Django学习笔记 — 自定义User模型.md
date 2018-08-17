# Django学习笔记 — 自定义User模型

最近做毕业设计，需要用到django和django rest framework，但是之前没写过django相关项目，只是看了一下，现在真正写起代码来各种问题呀。

由于我需要的User模型与django自带的User有所不同，所以需要定义自己的User Model，这里记录一下方法，适用于django 1.5+。

### **定义MyUserManager和MyUser**

修改myapp下的models.py文件：

```Python
from django.db import models
from django.contrib.auth.models import (
    BaseUserManager, AbstractBaseUser, PermissionsMixin)

class MyUserManager(BaseUserManager):
    def _create_user(self, username, email, password, **extra_fields):
        """
        Creates and saves a User with the given username, email and password.
        """
        if not username:
            raise ValueError('The given username must be set')
        email = self.normalize_email(email)
        user = self.model(username=username, email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_user(self, username, email, password, **extra_fields):
        extra_fields.setdefault('is_staff', False)
        return self._create_user(username, email, password, **extra_fields)

    def create_superuser(self, username, email, password, **extra_fields):
        extra_fields.setdefault('is_staff', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('Superuser must have is_staff=True')

        return self._create_user(username, email, password, **extra_fields)

class MyUser(AbstractBaseUser, PermissionsMixin):
    username = models.CharField(max_length=254, unique=True, db_index=True)
    email = models.EmailField('email address', max_length=254)

    is_staff = models.BooleanField('staff status', default=False)
    is_active = models.BooleanField('active', default=True)

    USERNAME_FIELD = 'username'
    REQUIRED_FIELDS = ['email']

    objects = MyUserManager()

    class Meta:
        db_table = 'myuser'

    def get_full_name(self):
        return self.username

    def get_short_name(self):
        return self.username123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

这里的MyUserManager和django的UserManager大同小异，也可以直接继承UserManager，然后修改_create_user函数即可。

MyUser类即为我们自定义的User模型，我们可以根据需要添加各种属性。

### **修改settings.py**

修改settings.py文件，添加如下内容设置认证使用的model：

```
AUTH_USER_MODEL = 'myapp.MyUser'

```

### **更新数据库**

首先删掉之前的数据库，然后重新建立，运行如下命令生成新的数据表：

```
$ python manage.py makemigrations myapp
$ python manage.py migrate

```

经过这三步，默认的User模型已经被替换成了我们自己定义的User模型了。当然我们也可以定义自己的认证模型以及权限系统，后面涉及到相关部分再添加笔记了。

版权声明：本文为博主原创文章，转载请注明出处。 https://blog.csdn.net/wangtaoking1/article/details/50187589