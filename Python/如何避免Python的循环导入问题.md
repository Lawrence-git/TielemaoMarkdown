# 如何避免Python的循环导入问题

https://blog.igevin.info/posts/how-to-avoid-python-circle-import-error/

Python 中使用package时，出现循环导入问题十分常见，我们创建如下package来说明这个问题:

```
pkg
  ├── __init__.py
  ├── module_a.py
  └── module_b.py

```

其中，

*   `[__init__.py](http://__init__.py)` 将`pkg`指定为一个Python package
*   `[module_a.py](http://module_a.py)`中定义了一个`action_a()`函数，该函数引用了`[module_b.py](http://module_b.py)`中的一个attribute，如一个函数或变量
*   `[module_b.py](http://module_b.py)`中定义了一个`action_b()`函数，该函数引用了`[module_a.py](http://module_a.py)`中的一个attribute，如一个函数或变量

这种情况下，执行该package时会抛出**circular import error**错误，即循环引用，因为`module_a`试图去引入`module_b`时，而`module_b`首先要引入`module_a`，这会导致Python解释器无法执行下去。

然而，我们可以通过一些巧妙的方法，让上面的逻辑正常工作，同时避免循环引入的错误。

那么，什么时候它能正常工作，什么时候不能正常工作，而那些能够正常工作的情况又是什么原因呢？

## 何时它能正常工作？

### 1\. 在module顶部引入，不要用`from`，相对引入，只在Python 2中有效

在module的顶部import，如`import another_module`，module 中的函数以`another_module.attribute`的方式引用`another_module`中的函数或变量等。这种方式之所以有效，是由于`import another_module`是基于当前目录的相对引用，而且是一种隐式引用，如果从另一个package中引入module时，就可以失效了。另外，`import another_module`这种语法在Python 3 中已经不支持了，所以不要在代码中用这种方法来避免循环引入。

如：

```
# pkg/module_a.py 
from __future__ import print_function
import module_b

def action_a():
    print(module_b.action_b.__name__)

# pkg/module_b.py
from __future__ import print_function
import module_a

def action_b():
    print(module_a.action_a.__name__)

```

### 2\. 在module的顶部引入，不要用`from`，绝对引入

在module的顶部import，使用从package开始的绝对路径，如`import package.another_module`，module 中的函数以`package.another_module.attribute`的方式引用`another_module`中的函数或变量等。之所以要挂上package name来引入，是由于`import .another_module`这种形式的“相对引入”会报语法错误，而挂上package的绝对引入，Python 2和3都支持

案例：

```
# pkg/module_a.py
from __future__ import print_function
import pkg2.module_b

def action_a():
    print(pkg2.module_b.action_b.__name__)

# pkg/module_b.py
from __future__ import print_function
import pkg2.module_a

def action_b():
    print(pkg2.module_a.action_a.__name__)

```

### 3\. 在module底部引入another module的attribute，而非another module，用`from`

在module的底部import（至少要在被引用的attribute之后import），直接引入another module的attribute，如`from package.another_module import attribute`，相对引入也支持，如`from .another_module import attribute`，module中的函数直接使用被引用的attribute即可。

如：

```
# pkg/module_a.py
from __future__ import print_function

def action_a():
    print(action_b.__name__)

from .module_b import action_b

# pkg/module_b.py
from __future__ import print_function

def action_b():
    print(action_a.__name__)

from .module_a import action_a

```

### 4\. 函数顶部引入，可以用`from`

在module的**function**顶部import，如`from package import another_module`，也支持相对引入，引入module或attribute均可。

如：

```
# pkg/module_a.py
from __future__ import print_function

def action_a():
    from . import module_b
    print(module_b.action_b.__name__)

# pkg/module_b.py
from __future__ import print_function

def action_b():
    from . import module_a
    print(module_a.action_a.__name__)

```

或

```
# pkg/module_a.py
from __future__ import print_function

def action_a():
    from .module_b import action_b
    print(action_b.__name__)

# pkg/module_b.py
from __future__ import print_function
def action_b():
    from .module_a import action_a
    print(action_a.__name__)

```

这种方式虽然Python 2和3都支持，但编码不够优雅，影响代码可读性，不建议使用

# 注

*   本文讨论的问题，是Python中调用package时，应如何避免循环引入
*   当直接在命令行执行一个Python module时，适用情况不完全相同
*   本文内容我在GitHub上提供了一个[**Demo**](https://github.com/flyhigher139/python_circular_import)，欢迎查看或fork
*   Reference：[**This Gist**](https://gist.github.com/datagrok/40bf84d5870c41a77dc6)