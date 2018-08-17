# Django之F和Q查询
http://blog.51cto.com/daibaiyang119/2046959

当一般的查询语句已经无法满足我们的需求时，Django为我们提供了F和Q复杂查询语句。假设场景一：老板说对数据库中所有的商品，在原价格的基础上涨价10元，你该怎么做？场景二：我要查询一个名字叫xxx，年龄是18岁，或者名字是yyy，年龄是是19岁的人，你该怎么写你的ORM语句？

## 一、F查询   

```python
from django.db.models import F
from app01.models import Book

Book.objects.update(price=F("price")+20)  # 对于book表中每本书的价格都在原价格的基础上增加20元
```

就这样一条简单的语句就完成了对表中所有商品价格的更新，是不是很方便！如果没有F查询，你首先要获取原价格，再做一个算术运算，然后更新字段。F查询专门对对象中某列值的操作，不可使用__双下划线！

## 二、Q查询

Q查询可以组合使用 “&”, “|” 操作符，当一个操作符是用于两个Q的对象,它产生一个新的Q对象，Q对象可以用 “~” 操作符放在前面表示否定，也可允许否定与不否定形式的组合。Q对象可以与关键字参数查询一起使用，不过一定要把Q对象放在关键字参数查询的前面。

```python
from django.db.models import Q

print(Book.objects.filter(Q(id=3))[0])  # 因为获取的结果是一个QuerySet，所以使用下标的方式获取结果
print(Book.objects.filter(Q(id=3)|Q(title="Go"))[0])  # 查询id=3或者标题是“Go”的书
print(Book.objects.filter(Q(price__gte=70)&Q(title__startswith="J")))  # 查询价格大于等于70并且标题是“J”开头的书
print(Book.objects.filter(Q(title__startswith="J") & ~Q(id=3)))  # 查询标题是“J”开头并且id不是3的书
print(Book.objects.filter(Q(price=70)|Q(title="Python"), publication_date="2017-09-26"))  # Q对象可以与关键字参数查询一起使用，必须把普通关键字查询放到Q对象查询的后面
```

```python
from django.db.models import Q

con = Q()
q1 = Q()
q1.connector = "AND"
q1.children.append(("email", "123@qq.com"))
q1.children.append(("password", "abc123"))

q2 = Q()
q2.connector = "AND"
q2.children.append(("username", "abc"))
q2.children.append(("password", "xyz123"))

con.add(q1, "OR")
con.add(q2, "OR")

obj = models.UserInfo.objects.filter(con).first()

# 查询email=123@qq.com和password=abc123 或者 username=abc和password=xyz123的用户信息
```

上面的例子就是一个典型的复杂查询，通过将Q对象实例化来然后增加各个条件之间的关系，而且这种写法用在你不知道用户到底会传入多少个参数的时候很方便！