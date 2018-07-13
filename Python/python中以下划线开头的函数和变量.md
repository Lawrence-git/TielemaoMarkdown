# python中以下划线开头的函数和变量

[[toc]]

Python用下划线作为变量前缀和后缀来指定特殊变量。

* `_xxx` 不能用`from module import *`导入

* `__xxx__` 系统定义名字

* `__xxx` 类中的私有变量名

下划线对解释器有特殊的意义，而且是内建标识符所使用的符号，程序员应避免用下划线作为变量名的开始。

* `单下划线` 开始的成员变量叫做保护变量，意思是只有类对象和子类对象自己能访问到这些变量；
	
	* 以单下划线开头（`_foo`）的代表 不能直接访问的类属性，需通过类提供的接口进行访问，不能用`from xxx import *`导入；
	
	* 一般而言，变量名`_xxx`被看作是`私有的`，在模块或类外不可以使用。
	
	* 当变量是私有的时候，用`_xxx`来表示变量是很好的习惯。

* `双下划线` 开始的是私有成员，意思是只有类对象自己能访问，连子类对象也不能访问到这个数据。

	* 以双下划线开头的（`__foo`）代表类的私有成员；

* 以双下划线开头和结尾的（`__foo__`）代表python里特殊方法专用的标识，如 `__init__（）`代表类的构造函数。

## 系统定义的属性和方法

### 保留属性：

```python
>>> Class.__doc__      # 类帮助信息 
>>> Class.__name__     # 类名
>>> Class.__module__   # 类所在模块 '__main__' 
>>> Class.__bases__    # 类所继承的基类 
>>> Class.__dict__     # 类字典，存储所有类型成员信息。 
>>> Class().__class__  # 类型 <class '__main__.Class'> 
>>> Class().__module__ # 实例类型所在模块 '__main__' 
>>> Class().__dict__   # 对象字典，存储所有实例成员信息。 
```
### 保留方法：

#### 类的基础方法

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
| ① | 初始化一个实例 | `x = MyClass()` | `x.__init__()`|
| ② | 字符串的“官方”表现形式 | `repr(x)` | `x.__repr__()`|
| ③ | 字符串的“非正式”值 |`str(x)`| `x.__str__()` |
| ④ | 字节数组的“非正式”值 | `bytes(x)` | `x.__bytes__()` |
| ⑤ | 格式化字符串的值 | `format(x, format_spec)` | `x.__format__(format_spec)` |

* 1. 对 `__init__()` 方法的调用发生在实例被创建之后。
如果要控制实际创建进程，请使用 `__new__()` 方法。
* 2. 按照约定， `__repr__()` 方法所返回的字符串为合法的 Python 表达式。
* 3. 在调用 `print(x)` 的同时也调用了 `__str__()` 方法。
* 4. 从 Python 3 开始引入出现`bytes` 类型。

## 行为方式与迭代器类似的类

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
| ① | 遍历某个序列 | `iter(seq)` | `seq.__iter__()`|
| ② | 从迭代器中获取下一个值 | `next(seq)` | `seq.__next__()` |
| ③ | 按逆序创建一个迭代器 | `reversed(seq)` | `seq.__reversed__()` |

* 1. 无论何时创建迭代器都将调用 `__iter__()` 方法。这是用初始值对迭代器进行初始化的绝佳之处。
* 2. 无论何时从迭代器中获取下一个值都将调用 `__next__()` 方法。
* 3. `__reversed__()` 方法并不常用。它以一个现有序列为参数，并将该序列中所有元素从尾到头以逆序排列生成一个新的迭代器。

## 计算属性

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
| ① | 获取一个计算属性（无条件的） | `x.my_property` | `x.__getattribute__('my_property')`|
| ② | 获取一个计算属性（后备） | `x.my_property` | `x.__getattr__('my_property')`|
| ③ | 设置某属性 | `x.my_property = value` | `x.__setattr__('my_property',value)`|
| ④ | 删除某属性 | `del x.my_property` | `x.__delattr__('my_property')`|
| ⑤ | 列出所有属性和方法 | `dir(x)` | `x.__dir__()`|

* 1. 如果某个类定义了 `__getattribute__()` 方法，在 每次引用属性或方法名称时Python 都调用它（特殊方法名称除外，因为那样将会导致讨厌的无限循环）。
* 2. 如果某个类定义了 `__getattr__()` 方法，Python 将只在正常的位置查询属性时才会调用它。如果实例 x 定义了属性color， `x.color` 将不会调用`x.__getattr__('color')`；而只会返回x.color 已定义好的值。
* 3. 无论何时给属性赋值，都会调用 `__setattr__()` 方法。
* 4. 无论何时删除一个属性，都将调用 `__delattr__()` 方法。
* 5. 如果定义了 `__getattr__()` 或 `__getattribute__()` 方法， `__dir__()` 方法将非常有用。通常，调用 `dir(x)` 将只显示正常的属性和方法。如果`__getattr()__`方法动态处理color 属性， `dir(x)` 将不会将 color 列为可用属性。可通过覆盖 `__dir__()` 方法允许将 color 列为可用属性，对于想使用你的类但却不想深入其内部的人来说，该方法非常有益。

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
|   | 序列的长度 | `len(seq)` | `seq.__len__()`|
|   | 了解某序列是否包含特定的值 | `x in seq` | `seq.__contains__(x)`|

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
|   | 通过键来获取值 | `x[key]` | `x.__getitem__(key)`|
|   | 通过键来设置值 | `x[key] = value` | `x.__setitem__(key,value)`|
|   | 删除一个键值对 | `del x[key]` | `x.__delitem__(key)` |
|   | 为缺失键提供默认值 | `x[nonexistent_key]` | `x.__missing__(nonexistent_key)` |

## 可比较的类

“比较”操作并不局限于数字。许多数据类型都可以进行比较——字符串、列表，甚至字典。如果要创建自己的类，且对象之间的比较有意义，可以使用下面的特殊方法来实现比较。

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
|   | 相等 | `x == y` | `x.__eq__(y)`|
|   | 不相等 | `x != y` | `x.__ne__(y)`|
|   | 小于 | `x < y` | `x.__lt__(y)`|
|   | 小于或等于 | `x <= y` | `x.__le__(y)` |
|   | 大于 | `x > y` | `x.__gt__(y)`|
|   | 大于或等于 | `x >= y` | `x.__ge__(y)` |
|   | 布尔上上下文环境中的真值 | `if x:` | `x.__bool__()` |

## 可序列化的类

Python 支持 `任意对象的序列化和反序列化`。（多数 Python 参考资料称该过程为 “pickling” 和 “unpickling”）。该技术对与将状态保存为文件并在稍后恢复它非常有意义。所有的 `内置数据类型` 均已支持 pickling 。如果创建了自定义类，且希望它能够 pickle，阅读 pickle 协议了解下列特殊方法何时以及如何被调用。

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
|   | 自定义对象的复制 | `copy.copy(x)` | `x.__copy__()`|
|   | 自定义对象的深度复制 | `copy.deepcopy(x)` | `x.__deepcopy__()` |
|   | 在 pickling 之前获取对象的状态 | `pickle.dump(x, file)` | `x.__getstate__()`|
|   | 序列化某对象 | `pickle.dump(x, file)` | `x.__reduce__()`|
|   | 序列化某对象（新 pickling 协议） | `pickle.dump(x, file, protocol_version)` | `x.__reduce_ex__(protocol_version)`|
| * | 控制 unpickling 过程中对象的创建方式 | `x = pickle.load(file)` | `x.__getnewargs__()`|
| * | 在 unpickling 之后还原对象的状态 | `x = pickle.load(file)` | `x.__setstate__()`|

* 要重建序列化对象，Python 需要创建一个和被序列化的对象看起来一样的新对象，然后设置新对象的所有属性。`__getnewargs__()` 方法控制新对象的创建过程，而 `__setstate__()` 方法控制属性值的还原方式。

## 可在 `with` 语块中使用的类

`with` 语块定义了 `运行时刻上下文环境`；
在执行 `with` 语句时将“进入”该上下文环境，而执行该语块中的最后一条语句将“退出”该上下文环境。

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
|   | 在进入 `with` 语块时进行一些特别操作 | `with x:` | `x.__enter__()` |
|   | 在退出 `with` 语块时进行一些特别操作 | `with x:` | `x.__exit__()`|

以下是 `with file` 习惯用法 的运作方式：

``` python
# excerpt from io.py: 
def _checkClosed(self, msg=None):  
   
'''Internal: raise an ValueError if file is closed  '''
	if self.closed:   
		raise ValueError('I/O operation on closed file.' if msg is None else msg)  

def __enter__(self):
   
'''Context management protocol.  Returns self.'''  
	self._checkClosed()                       ①
	return self                               ②  

def __exit__(self, *args):

'''Context management protocol.  Calls close()'''
	self.close()                                       ③
```

* 1.  该文件对象同时定义了一个 `__enter__()` 和一个 `__exit__()` 方法。
该 `__enter__()` 方法检查文件是否处于打开状态；如果没有， `_checkClosed()`方法引发一个例外。
* 2.  `__enter__()` 方法将始终返回 `self` —— 这是 `with` 语块将用于调用属性和方法的对象。
* 3.  在 `with` 语块结束后，文件对象将自动关闭。
怎么做到的？在 `__exit__()` 方法中调用了 `self.close()` .
该 `__exit__()` 方法将总是被调用，哪怕是在 `with` 语块中引发了例外。
实际上，如果引发了例外，该例外信息将会被传递给 `__exit__()` 方法。

**真正神奇的东西**

如果知道自己在干什么，你几乎可以完全控制类是如何比较的、属性如何定义，以及类的子类是何种类型。

| 序号 | 目的 | 所编写代码 | Python 实际调用 |
|-|-|-|-|
|   | 类构造器 | `x = MyClass()` | `x.__new__()` |
| * | 类析构器 | `del x` | `x.__del__()`|
|   | 只定义特定集合的某些属性 |   | `x.__slots__()`|
|   | 自定义散列值 | `hash(x)` | `x.__hash__()`|
|   | 获取某个属性的值 | `x.color` | `type(x).__dict__['color'].__get__(x, type(x))`|
|   | 设置某个属性的值 | `x.color = 'PapayaWhip'` | `type(x).__dict__['color'].__set__(x, 'PapayaWhip')`|
|   | 删除某个属性 | `del x.color` | `type(x).__dict__['color'].__del__(x)` |
|   | 控制某个对象是否是该对象的实例 your class | `isinstance(x, MyClass)` | `MyClass.__instancecheck__(x)`|
|   | 控制某个类是否是该类的子类 | `issubclass(C, MyClass)` | `MyClass.__subclasscheck__(C)` |
|   | 控制某个类是否是该抽象基类的子类 | `issubclass(C, MyABC)` | `MyABC.__subclasshook__(C)`|

python中以双下划线的是一些系统定义的名称，让python以更优雅得语法实行一些操作，本质上还是一些函数和变量，与其他函数和变量无二。
比如`x.__add__(y)` 等价于 `x+y`

简记：

* `x.__contains__(y)` 等价于 `y in x`, 在list,str, dict,set等容器中有这个函数。
* `__base__`, `__bases__`, `__mro__`, 关于类继承和函数查找路径的。
* `class.__subclasses__()`, 返回子类列表
* `x.__call__(...) == x(...)`
* `x.__cmp__(y) == cmp(x,y)`
* `x.__getattribute__('name') == x.name == getattr(x, 'name')`,  比`__getattr__`更早调用
* `x.__hash__() == hash(x)`
* `x.__sizeof__()`, x在内存中的字节数, x为class得话， 就应该是`x.__basicsize__`
* `x.__delattr__('name') == del x.name`
* `__dictoffset__` attribute tells you the offset to where you find the pointer to the `__dict__` object in any instance object that has one. It is in bytes.
*`__flags__`, 返回一串数字，用来判断该类型能否被序列化（if it's a heap type), `__flags__` & 512
* `S.__format__`, 有些类有用
* `x.__getitem__(y) == x[y]`, 相应还有`__setitem__`, 某些不可修改类型如set，str没有`__setitem__`
* `x.__getslice__(i, j) == x[i:j]`
* `__subclasscheck__()`, check if a class is subclass
* `__instancecheck__()`, check if an object is an instance
* `__itemsize__`, These fields allow calculating the size in bytes of instances of the type. 
0是可变长度， 非0则是固定长度
* `x.__mod__(y) == x%y`, `x.__rmod__(y) == y%x`
* `x.__module__ `, x所属模块
* `x.__mul__(y) == x*y`,  `x.__rmul__(y) == y*x`
* `__reduce__`, `__reduce_ex__` , for pickle
* `__slots__` 使用之后类变成静态一样，没有了`__dict__`, 实例也不可新添加属性
* `__getattr__` 在一般的查找属性查找不到之后会调用此函数
* `__setattr__` 取代一般的赋值操作，如果有此函数会调用此函数， 如想调用正常赋值途径用 *`object.__setattr__(self, name, value)`
* `__delattr__` 同`__setattr__`, 在del obj.name有意义时会调用

