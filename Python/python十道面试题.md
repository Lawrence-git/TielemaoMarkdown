**问题1:** 请问如何修改以下python代码，使得下面的代码调用类A的show方法？

```python
class A():

    def show(self):
        print("base show")

class B(A):

    def show(self):
        print("derived show")

obj = B()
obj.show()

```

答：这道题的考点是类继承，只要通过__class__方法指定类对象就可以了。修改如下，其实就是只补充了一行`obj.__class__ = A`代码：

```python
class A():

    def show(self):
        print("base show")

class B(A):

    def show(self):
        print("derived show")

obj = B()
obj.__class__ = A
obj.show()

```

**问题2**：请问如何修改以下python代码，使得代码能够运行？

```python
class A():

    def __init__(self, a, b):
        self.__a = a
        self.__b = b

    def myprint(self):
        print('a =', self.__a, 'b =', self.__b)


a1 = A(10, 20)
a1.myprint()

a1(80)
```

答：此题考察的是方法对象，为了能让对象实例能被直接调用，需要实现 `__call__` 方法，补充代码如下：

```python
    def __call__(self, num):
        print('call:', num + self.__a)
```
调用加a的值只是一个示例，也可以加b的值等，题目主要考的是实现call调用。
完整代码如下：

```python
class A():

    def __init__(self, a, b):
        self.__a = a
        self.__b = b

    def myprint(self):
        print('a =', self.__a, 'b =', self.__b)

    def __call__(self, num):
        print('call:', num + self.__a)


a1 = A(10, 20)
a1.myprint()

a1(80)
```

**问题3：** 下面这段代码的输出是什么？

```python
class B():

    def fn(self):
        print('B fn')

    def __init__(self):
        print('B INIT')


class A():

    def fn(self):
        print('A fn')

    def __new__(cls, a):
        print("NEW", a)
        if a > 10:
            return super(A, cls).__new__(cls)
        return B()

    def __init__(self, a):
        print("INIT", a)


a1 = A(5)
a1.fn()
a2 = A(20)
a2.fn()
```

答：
输出的结果为
```python
NEW 5
B INIT
B fn
NEW 20
INIT 20
A fn
```
此题考察的是new和init的用法，使用 `__new__ `方法，可以决定返回那个对象，也就是创建对象之前调用的，这个常见于于设计模式的单例、工厂模式。
`__init__ `是创建对象是调用的。

**问题4:** 下面这段代码输出什么？

```python
ls = [1, 2, 3, 4]
list1 = [i for i in ls if i > 2]
print(list1)

list2 = [i * 2 for i in ls if i > 2]
print(list2)

dic1 = {x: x ** 2 for x in (2, 4, 6)}
print(dic1)

dic2 = {x: 'item' + str(x ** 2) for x in (2, 4, 6)}
print(dic2)

set1 = {x for x in 'hello world' if x not in 'low level'}
print(set1)
```

答：这题算简单，没有什么陷阱，只是单纯的考察你对列表和字典的生成式的掌握。
输出结果为：
```python
[3, 4]
[6, 8]
{2: 4, 4: 16, 6: 36}
{2: 'item4', 4: 'item16', 6: 'item36'}
{'r', 'h', 'd'}
```

**问题5：** 下面这段代码输出什么?

```python
num = 9

def f1():
    num = 20

def f2():
    print(num)

f2()
f1()
f2()
```

答：输出的是
```
9
9
```
注意中间的f1函数只是单纯的赋值，而没有print打印输出，我看完题第一感觉答的是`9，20，9`就聪明反被聪明误了。

此题考察全局变量和局部变量。f1函数中的num 不是全局变量，如果你想修改 num ，则必须用 global 关键字声明。

比如修改成下面这样的代码，就会输出的是9和20:

```python
num = 9

def f1():
    global num
    num = 20

def f2():
    print(num)

f2()
f1()
f2()
```

**问题6：** 如何使用一行代码交换两个变量值？
例：
```python
a = 8
b = 9
```

答：
```python
(a, b) = (b, a)
```

考的就是你代码够不够pythonic。

**问题7：** 如何添加代码，使得没有定义的方法都调用mydefault方法？

```python
class A():

    def __init__(self, a, b):
        self.a1 = a
        self.b1 = b
        print('init')

    def mydefault(self):
        print('default')

a1 = A(10, 20)
a1.fn1()
a1.fn2()
a1.fn3()
```

答：
```python
class A():

    def __init__(self, a, b):
        self.a1 = a
        self.b1 = b
        print('init')

    def mydefault(self):
        print('default')

    def __getattr__(self, item):
        return self.mydefault

a1 = A(10, 20)
a1.fn1()
a1.fn2()
a1.fn3()
```

此题的考的是Python的默认方法， 只有当没有定义的方法调用时，
才会调用方法 `__getattr__`。要注意的是该方法需要传入至少两个参数。
而当 fn1 方法需要传入参数时，我们可以给 mydefault 方法增加一个` *args `不定参数来兼容。

```python
class A():

    def __init__(self, a, b):
        self.a1 = a
        self.b1 = b
        print('init')

    def mydefault(self, *args):
        print('default:' + str(args[0]))

    def __getattr__(self, name):
        print("other fn:", name)
        return self.mydefault

a1 = A(10, 20)
a1.fn1(33)
a1.fn2('hello')
a1.fn3(10)
```

**问题8 :**  一个包里有三个模块，mod1.py，mod2.py , mod3.py ，
但使用 `from demopack import * `导入模块时，如何保证只有 mod1 、 mod3 被导入了。

答:在包中增加 `__init__.py` 文件，并在文件中增加：
`__all__  = ['mod1', 'mod3']`

**问题9：** 写一个函数，接收整数参数 n ，返回一个函数，函数返回n和参数的积。

答:

```python
def mulby(num):

    def gn(val):
        return num * val

    return gn

zw = mulby(7)
print(zw(9))
```
这一题答案虽然简单，但要读懂题意却颇难，考的是嵌套函数和返回值。

**问题10：** 请问下面的代码有什么隐患？（Python2中）

```python
def strtest1(num):
    str = 'first'
    for i in range(num):
        str += "X"
    return str
```

答：由于变量str是个不可变对象，每次迭代，python都会生成新的str对象来存储新的字符串，num越大，创建的str对象越多，内存消耗越大。

【end】

