# Django REST FrameWork中文教程1:序列化

@toc

## 前言
由于这个教程原本是英文的，而且有些年头了，所以我自行重新组织了语言去介绍这个教程。
以及你将会看到我在windows平台上是如何学习这个教程的。
本教程将涵盖一个简单的PasteBin1代码高亮的Web API。
整个过程，将逐一介绍REST framework的各个组成部件，让你全面理解，组件之间是如何整合的。

**注意**
* 此教程我是在windows7平台下学习和演示的，有些命令和路径会与原教程的linux系统不同。
* 原作者的代码可以在Github中找到：[tomchristie/rest-framework-tutorial](https://github.com/tomchristie/rest-framework-tutorial)。完整的代码部署在线上的沙盒版本（sand version）里，用作[测试](http://restframework.herokuapp.com/)。

## 搭建虚拟环境

准备安装以下软件版本来学习
* django 2.1
* django-filter==2.0.0  
  * 过滤
* python 3.5
* pip 18.0
  * 主要就是靠它在虚拟环境下安装包了
* Markdown==2.6.11
  * Markdown为可视化 API 提供了支持.
* django-crispy-forms==1.7.2
  * 为过滤，提供了改良的HTML呈现.
* virtualenv 16.0.0
* djangorestframework 3.8.2
* pygments 2.2.0
  * 实现代码高亮 

在开始之前，先使用virtualenv创建一个新的虚拟环境。 
这将确保我们的软件包配置与我们正在进行的其他任何项目保持良好的隔离。

```bin
# 建立项目
mkdir learn_restframework
cd learn_restframework

# 命名和创建虚拟环境
virtualenv env

# 如果是在linux系统，将敲以下命令去激活虚拟环境
source env/bin/activate

# 当然由于我是在win平台下做的虚拟环境，所以目录路径会有所不同，直接去到目录下执行activate即可
env\Scripts>activate
```
 
进入virtualenv环境后可以开始安装需要的软件支持包。

```
pip install django=1.9
pip install djangorestframework
pip install pygments  # 实现代码高亮
```

**注意：**想要随时退出virtualenv环境，只需输入`deactivate`。欲了解更多信息，请参阅[virtualenv文档](http://www.virtualenv.org/en/latest/index.html)。

## 创建项目和注册app

* 创建一个新项目（project），命名为tutorial，教程的意思。

```
cd learn_restframework # 返回之前己准备好的项目目录
# linux下命令： django-admin.py startproject tutorial

# windows为直接找到django-admin.exe执行
.\env\Scripts\django-admin.exe startproject tutorial

cd tutorial
```

* 创建一个app应用程序，命名为snippets（片段的意思），来创建一个简单的Web API。
```
python manage.py startapp snippets
```
将新建的snippets应用和`rest_framework`应用添加到django的settings配置`INSTALLED_APPS`中。 
编辑`tutorial/settings.py`文件：
```
INSTALLED_APPS = (
    ...
    'rest_framework',
    'snippets.apps.SnippetsConfig',
)
```
**注意** 如果你使用的Django < 2.0，则需要将snippets.apps.SnippetsConfig更换为snippets。

### 附URL

如果你需要使用可视化的API，你也许就需要添加REST Framework的登陆/登出视图。在你的根 `urls.py` 文件里，添加下面的内容：

```
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework')),
]
```

需要留意的是，URL路径可以任意编写，但你必须include `'rest_framework.urls'`，且使用`'rest_framework'`这个namespace(命名空间)。如果你的Django是1.9+版本，你也可以不写namespace，REST framework会帮你自动设置。

### 举例

让我们看一个简单用例：如何用REST framework 来搭建一个简单的支持modle的API。
我们将创建一个读/写API，来处理我们项目中的用户信息。
任何REST framework的全局设置，都存放在一个配置字典（dictionary，有些语言如java中的map）中，名为`REST_FRAMEWORK`。我们从以下的操作开始，把下面的内容添加到你的`settings.py`模块中：

```
REST_FRAMEWORK = {
    # 使用Django的标准`django.contrib.auth`权限管理类,
    # 或者为尚未认证的用户，赋予只读权限.
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.DjangoModelPermissionsOrAnonReadOnly'
    ]
}
```

别忘了，确保你已经将 `rest_framework` 添加到你的`INSTALLED_APPS`中。
现在我们已做好准备，来创建我们的API了。这是我们的项目根下`urls.py`模块：

```python
from django.conf.urls import url, include
from django.contrib.auth.models import User
from rest_framework import routers, serializers, viewsets

# Serializers定义了API的表现.
class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ('url', 'username', 'email', 'is_staff')

# ViewSets 定义了 视图（view） 的行为.
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer

# Routers 提供了一种简单途径，自动地配置了URL。
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)

# 使用自动的URL路由，让我们的API跑起来。
# 此外，我们也包括了登入可视化API的URLs。
urlpatterns = [
    url(r'^', include(router.urls)),
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```
现在你可以在浏览器的**[http://127.0.0.1:8000/](http://127.0.0.1:8000/)**里，打开你新建的`users`API了。使用右上角的登陆控制，可以对系统用户进行新增和删除操作。


## 创建模型（model）

处于教程的设计考虑，我们首先创建一个简单`Snippets`模型，用来存储相关代码。
注意：良好的编程实践会有注释。 

编辑`snippets/models.py`文件
```python
from django.db import models
from pygments import highlight
from pygments.formatters.html import HtmlFormatter
from pygments.lexers import get_all_lexers, get_lexer_by_name
from pygments.styles import get_all_styles

LEXERS = [item for item in get_all_lexers() if item[1]]
LANGUAGE_CHOICES = sorted([(item[1][0], item[0]) for item in LEXERS])
STYLE_CHOICES = sorted((item, item) for item in get_all_styles())

class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    code = models.TextField()
    linenos = models.BooleanField(default=False)
    language = models.CharField(choices=LANGUAGE_CHOICES, default='python', max_length=100)
    style = models.CharField(choices=STYLE_CHOICES, default='friendly', max_length=100)
    owner = models.ForeignKey('auth.User',related_name='snippets',on_delete=models.CASCADE)
    highlighted = models.TextField()

    # Model元数据就是“ 不是一个字段的任何数据 ”
    class Meta:
        # ordering告诉Django模型对象返回的记录结果集是按照哪个字段排序的
        ordering = ('created',)

    def save(self, *args, **kwargs):
        """
        Use the 'pygments' library to create a highlighted HTML representation of the code snippet.        
        """  
        lexer = get_lexer_by_name(self.language)
        linenos = self.linenos and 'table' or False
        options = self.title and {'title': self.title} or {}
        formatter = HtmlFormatter(style=self.style, linenos=linenos,full=True,**options)
        self.highlighted = highlight(self.code, lexer, formatter)
        super(Snippet, self).save(*args, **kwargs)
```

为snippet模型创建好数据表后，
将模型同步到数据库中，实现初始的迁移（migration）。
```
python manage.py makemigrations snippets
python manage.py migrate
```

## 创建序列化Serializer类

我们的 Web API 将开始于，为代码片段的实例（instances）提供序列化和反序列化的途径，使之可以转化为某种表现形式如 `json` 。
可以借助声明序列器（serializer）来实现，类似于Django表单（form）的运作方式。
简而言之，可以直接理解为Serializer可以帮我们做序列化，一般是转化为json格式，当然其实不止是json。
在 `snippets` 路径下，创建文件 `serializers.py` 并敲入以下内容。

```python
from rest_framework import serializers
from snippets.models import Snippet, LANGUAGE_CHOICES, STYLE_CHOICES

class SnippetSerializer(serializers.Serializer):
    pk = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)
    language = serializers.ChoiceField(choices=LANGUAGE_CHOICES, default='python')
    style = serializers.ChoiceField(choices=STYLE_CHOICES, default='friendly')

    def create(self, validated_data):
        """
        传入验证过的数据, 创建并返回`Snippet`实例。
        """
        return Snippet.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """
        传入验证过的数据, 更新并返回已有的`Snippet`实例。
        """
        instance.title = validated_data.get('title', instance.title)
        instance.code = validated_data.get('code', instance.code)
        instance.linenos = validated_data.get('linenos', instance.linenos)
        instance.language = validated_data.get('language', instance.language)
        instance.style = validated_data.get('style', instance.style)
        instance.save()
        return instance
```

* 序列器(`serializer`)类的第一部分，告诉REST框架，哪些字段(field)，需要被序列化/反序列化。
* `create()` 和 `update()` 方法，定义了如何创建和修改，一个有内容的实例对象。这两个方法会在运行 `serializer.save()` 时，被调用。

* 序列器类非常类似Django的 Form 类，在多个字段中，也包含了类似的验证标识(validation flags)，如 `required` ，`max_length` 和 `default`。

* 字段标识（flag）也能控制序列器在特定情况下是如何呈现（displayed）的，比如需要渲染（rendering）成HTML上面的 `{'base_template': 'textarea.html'}` 标识，相当于在Django的 Form 类中使用 `widget=widgets.Textarea`。
这尤其在控制可视化API如何来呈现时，特别有用。
我们在后面的教程中，会看到这点。

* 事实上，之会的教程中我们可以看到，可以使用 `ModelSerializer` 类， 来节省一些时间。但现在，我们会保持序列器中，每个字段的清晰定义。

## 让序列器运作起来

在此，让我们先停一停，来熟悉一下，如何使用我们新建的序列器。让我们进入Django shell。
```
python manage.py shell
```

好了，导入一些包后，为了下步的运作，让我们创建几个代码片段吧

```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser

# 创建数据
snippet = Snippet(code='foo = "bar"\n')
snippet.save()

snippet = Snippet(code='print "hello, world"\n')
snippet.save()
```

现在我们有几个可用的代码片段实例了。
让我们看看如何来序列化其中一个实例。
```
###该代码是把刚刚保存的数据snippet对象，经过序列化保存成一个字典
snippet = Snippet(code='print "hello, world"\n')
snippet.save()

###
serializer = SnippetSerializer(snippet)
serializer.data
# {'id': 2, 'title': u'', 'code': u'print "hello, world"\n', 'linenos': False, 'language': u'python', 'style': u'friendly'}
```
此刻，我们将模型实例，转化成了Python的原生数据类型（native datatypes）。要完成序列化的流程，我们将data渲染成`json`。
```python
# 将字典转换成json格式
content = JSONRenderer().render(serializer.data)
content
# '{"id": 2, "title": "", "code": "print \\"hello, world\\"\\n", "linenos": false, "language": "python", "style": "friendly"}'
```
反序列化是相似的。 首先，我们将一个流解析为Python数据类型
```
# 将json转换成字典格式
from django.utils.six import BytesIO

stream = BytesIO(content)
data = JSONParser().parse(stream)
```
然后我们将该原生数据类型，转换成对象实例。
```
serializer = SnippetSerializer(data=data)
serializer.is_valid()    # 验证数据是否符合要求
# True
serializer.validated_data    # 验证后的数据
# OrderedDict([('title', ''), ('code', 'print "hello, world"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
serializer.save()    # 保存数据
# <Snippet: Snippet object>
```
注意API的工作形式是如此的相似。这种重复性的相似，会在我们的视图（`view`）中，用到序列器的时候，变得更加的明显。

除了模型实例，我们也可以将queryset序列化。只需在序列器的参数中加入 `many=True` 。
```
serializer = SnippetSerializer(Snippet.objects.all(), many=True)
serializer.data
# [OrderedDict([('id', 1), ('title', u''), ('code', u'foo = "bar"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 2), ('title', u''), ('code', u'print "hello, world"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 3), ('title', u''), ('code', u'print "hello, world"'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])]
```

**使用ModelSerializers**

在 `SnippetSerializer` 类中，重复了许多，在 `Snippet` 模型中的字段定义。如果我们能保持代码简洁，岂不是很好？

就像Django即提供了 `Form` 类，也提供了 `ModelForm` 类， REST framework也有 `Serializer` 类和 `ModelSerializer` 类。

来看看如何，使用 `ModelSerializer` 类，重构我们的序列器。再次打开 snippets/serializers.py ， 将 `SnippetSerializer` 类替换为：
```
class SnippetSerializer(serializers.ModelSerializer):
    # ModelSerializer和Django中ModelForm功能相似
    # Serializer和Django中Form功能相似
    class Meta:
        model = Snippet
        fields = ('id', 'title', 'code', 'linenos', 'language', 'style')
```
序列化程序有个很好的特性，，您可以通过打印序列化的属性。查看序列器对象中所有的字段。在Django shell中（即 `python manage.py shell` ）试试吧：
```
from snippets.serializers import SnippetSerializer
serializer = SnippetSerializer()
print(repr(serializer))
# SnippetSerializer():
#    id = IntegerField(label='ID', read_only=True)
#    title = CharField(allow_blank=True, max_length=100, required=False)
#    code = CharField(style={'base_template': 'textarea.html'})
#    linenos = BooleanField(required=False)
#    language = ChoiceField(choices=[('Clipper', 'FoxPro'), ('Cucumber', 'Gherkin'), ('RobotFramework', 'RobotFramework'), ('abap', 'ABAP'), ('ada', 'Ada')...
#    style = ChoiceField(choices=[('autumn', 'autumn'), ('borland', 'borland'), ('bw', 'bw'), ('colorful', 'colorful')...
```
注意：`ModelSerializer`类不会做任何特别神奇的事情，它们只是创建序列化器类的快捷方式：

*   自动地声明了一套字段

*   默认的实现了`create()`和`update()`方法

**使用我们的Serializer编写正常的Django视图**

来看看如何使用新建的序列器（Serializer）类来编写一些API视图。到此为止，我们还没有使用过REST framework其他的特性，我们只是编写一个普通的Django视图。

我们将从，创建一个HttpResponse的子类开始，这个子类会将任何data渲染并返回为 `json`。

编辑`snippets/views.py`文件，并添加以下内容。
```
from django.http import HttpResponse, JsonResponse
from django.views.decorators.csrf import csrf_exempt
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
```
我们API的根url，将会成为一个视图，显示所有现存的代码片段，或创建一个新的代码片段。

```python
@csrf_exempt
def snippet_list(request):
    """
    List all code snippets, or create a new snippet.
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = SnippetSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            # serializer.data 数据创建成功后所有数据
            return JsonResponse(serializer.data, status=201)
        # serializer.errors 错误信息
        return JsonResponse(serializer.errors, status=400)
```

注意，因为我们需要POST数据，到这个视图的客户端，并没有CSRF令牌（token），所以我们需要为该视图标记为 `csrf_exempt` 。你平时不会做这种事，实际上，相比起这个，REST framework 的视图有着更加合理的行为，但现在我们会这么操作。

我们也需要一个视图，来响应某个单独的代码片段，并且可以获取，更新和删除这个片段。

```python
@csrf_exempt
def snippet_detail(request, pk):
    """
    Retrieve, update or delete a code snippet.
    """
    try:
        snippet = Snippet.objects.get(pk=pk)
    except Snippet.DoesNotExist:
        return HttpResponse(status=404)
    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return JsonResponse(serializer.data)
    elif request.method == 'PUT':
        data = JSONParser().parse(request)
        serializer = SnippetSerializer(snippet, data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        return JsonResponse(serializer.errors, status=400)
    elif request.method == 'DELETE':
        snippet.delete()
        return HttpResponse(status=204)
```

最后，我们需要注册这些视图。创建`snippets/urls.py`文件：
```language
from django.conf.urls import url
from snippets import views

urlpatterns = [
    url(r'^snippets/$', views.snippet_list),
    url(r'^snippets/(?P<pk>[0-9]+)/$', views.snippet_detail),
]
```

我们也需要，注册到 `tutorial/urls.py` 文件的根url配置（root urlconf）中，来包含我们的snippets app的URLs。
```language
from django.conf.urls import url, include

urlpatterns = [
    url(r'^', include('snippets.urls')),
]

```

需要注意的是，此刻，有一些边缘事件（edge cases），我们没有相应的处理。如果我们发送杂乱的 `json`， 或一个请求使用了一种请求方法，是我们视图没有涵盖的（如modify），那么我们会出现500 “server error”的响应（response）。总之，现在我们暂时这么做。

**测试我们在Web API上的第一次访问**

现在我们可以启动一个服务我们的代码片段的示例服务器。

退出Django shell...

quit()

并启动Django的开发服务器。

python manage.py runserver

Validating models...

0 errors found
Django version 1.11, using settings 'tutorial.settings'
Development server is running at http://127.0.0.1:8000/
Quit the server with CONTROL-C.

另起一个终端，我们可以测试服务器。

我们可以使用[curl](http://curl.haxx.se/)或[httpie](https://github.com/jakubroztocil/httpie#installation)来测试我们的API 。Httpie是用Python编写的用户友好的http客户端。

您可以使用pip安装httpie：

pip install httpie

最后，我们可以得到所有片段的列表：
```
http http://127.0.0.1:8000/snippets/HTTP/1.1 200 OK...[
  {
    "id": 1,
    "title": "",
    "code": "foo = \"bar\"\n",
    "linenos": false,
    "language": "python",
    "style": "friendly"
  },
  {
    "id": 2,
    "title": "",
    "code": "print \"hello, world\"\n",
    "linenos": false,
    "language": "python",
    "style": "friendly"
  }]

```


或者我们可以通过引用其id来获取特定的代码段：
```language
http http://127.0.0.1:8000/snippets/2/

HTTP/1.1 200 OK
...
{
  "id": 2,
  "title": "",
  "code": "print \"hello, world\"\n",
  "linenos": false,
  "language": "python",
  "style": "friendly"
}
```

同样，您可以通过在Web浏览器中访问这些URL来显示相同的json。

**我们现在在哪**

目前为止，我们做得还行，我们做的序列化API感觉跟Django的Form API 比较相似，并且我们做了一些普通的Django视图。

我们的API视图，现在还没做啥特别的事情。除了响应了json之外，还有一些没能处理的边缘事件，但至少还是个能用的Web API。

我们将在[本教程的第2部分中](http://www.chenxm.cc/post/290.html)看到我们如何开始改进事情。