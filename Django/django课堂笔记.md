# django课堂笔记

日期：2018-7-2
内容主题 :
讲师：苑昊

* mysql子查询：以一个结果做为下一次查询的对象。

## 日志打印：Login放在setting.py
http://www.liujiangblog.com/course/django/176

## 基于双下划线__的跨表查询

join查询：把多表拼接成一个表，在生成的表上进行（单表）查询。
所以重点在于拼接多个表。（笛卡尔）

**正向查询按字段，反向查询按小写表名变量（一对多，多对多，一对一）**

**你得学会通知ORM引擎什么时候join表。**

SELECT "app01_pulish"."name" from app01_book INNER JOIN app01_publish ON (app01_book.publish_id = app01_publish.id) WHERE app01_book.title="西游记“

基表，左表

Book.objects.filter(title="西游记").values("publish__name")

Publish.objects.filter(book__title="西游记“).values("name")

* 查询西游记这本书籍的所有作者的姓名和年龄
    * 正向查询
    `Book.objects.filter(title="西游记").values("authors__name","authors__age")`
    * 反向查询
    `Author.objects.filter(book__title="西游记").values("name","age")`

* 查询alex女朋友的名字
    Author.objects.fifter(name='alex').values("ad__gf")
    AuthorDetail.objects.filter(author__name="alex").values("gf")
    这其中ad是ORM帮你直接关联了两张表的对象
 
 * 查询手机号以151开头的作者出版过的所有书籍名称及出版社名称
 
 基表为book

## 分组查询

单表.objects.values("group by 字段).annotate(统计函数(统计字段))

1/确定是否是多表分组
2 /如果是多表分组，确定分组条件（group by 哪一个字段）
3 语法
