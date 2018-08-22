# Django admin

## 创建超级用户
`python manage.py createsuperuser`

## 自定义类
如下图，可以自定义一个类，然后再注册的时候多传这个类作为第二个参数。
可以实现一些方便的管理功能。
![django-admin-class]($resource/django-admin-class.jpg)

代码：
```python
from django.contrib import admin
from rbac import models

class PermissionAdmin(admin.ModelAdmin):
    # 给管理后台增加显示的列
    list_display = ['title', 'url']
    # 直接在显示页面对列进行操作，并方便批量修改完成后再点保存
    list_editable = ['url']

# 注册的时候增加多一个自定义的类
admin.site.register(models.Permission,PermissionAdmin)
admin.site.register(models.Role)
admin.site.register(models.UserInfo)
```

效果如下：
![django-admin-editable]($resource/django-admin-editable.jpg)

如上两图，分别实现了显示多列和直接编辑的管理功能。