## mysql-索引

http://www.py3study.com/Article/details/id/314.html
发布时间：2018-06-20 10:03:25    编辑：Run

**索引的介绍**

**数据库中专门用于帮助用户快速查找数据的一种数据结构。类似于字典中的目录，查找字典内容时可以根据目录查找到数据的存放位置吗，然后直接获取**

**索引的作用**

**约束和加速查找**

**常见的几种索引:**

**单列：普通索引，唯一索引，主键索引**

**多列：联合索引（多列），比如：联合主键索引、联合唯一索引、联合普通索引**

**联合索引，也称之为组合索引**

**总结:**

单列：
唯一索引：
  加速查找 + unique(约束)可以为空
普通索引：
   仅有一个功能:加速查找
   create index ix_name on userinfo(name);

主键索引：
   加速查找+约束（不为空）
多列：
组合索引

**主键索引比普通索引快**

**无索引和有索引的区别以及建立索引的目的**

**无索引： 从前往后一条一条查询**

**有索引：创建索引的本质，就是创建额外的文件（某种格式存储，查询的时候，先去格外的文件找，定好位置，然后再去原始表中直接查询。但是创建索引越多，会对硬盘也是有损耗。**

**建立索引的目的：**

**a.额外的文件保存特殊的数据结构**

**b.查询快，但是插入更新删除依然慢**

**c.创建索引之后，必须命中索引才能有效**

**索引的种类**

**hash索引和BTree索引**

**（1）hash类型的索引：查询单条快，范围查询慢**

**（2）btree类型的索引：b+树，层数越多，数据量指数级增长（我们就用它，因为innodb默认支持它）**

**总结:**

Hash索引
优点：单条数据查询速度要快
缺点： > < like 查询速度不一定快，因为hash索引生成hash值的是无序的，所以不能使用排序

BTREE索引
innodb引擎 默认是Btree索引，这个是根据二分查找查询

**普通索引**

**作用：仅有一个加速查找**

**创建表+普通索引**

create table userinfo(
   nid int not null auto_increment primary key,
   name varchar(32) not null,
   email varchar(64) not null,
   index ix_name(name)
);

**创建普通索引**

create index 索引的名字 on 表名(列名)

**删除普通索引**

drop index 索引的名字 on 表名

**查看索引**

show index from 表名

**唯一索引**

**唯一索引有两个功能：加速查找和唯一约束（可含null）**

**创建表+唯一索引**

create table userinfo(
   id int not null auto_increment primary key,
   name varchar(32) not null,
   email varchar(64) not null,
   unique  index  ix_name(name)
);

**创建唯一索引**

create unique index 索引名 on 表名(列名)

**删除唯一索引**

drop unique index 索引名 on 表名

**主键索引**

**主键索引有两个功能： 加速查找和唯一约束（不含null）**

**创建表+主键索引**

create table userinfo(
    id int not null auto_increment primary key,
    name varchar(32) not null,
    email varchar(64) not null,
    unique  index  ix_name(name)
);
或者
create table userinfo(
    id int not null auto_increment,
    name varchar(32) not null,
    email varchar(64) not null,
    primary key(id),
    unique  index  ix_name(name)
);

**创建主键索引**

alter table 表名 add primary key(列名);

**删除主键索引**

alter table 表名 drop primary key;
alter table 表名  modify  列名 int, drop primary key;

**组合索引**

**组合索引是将n个列组合成一个索引**

**其应用场景为：频繁的同时使用n列来进行查询，如：where name = 'sam' and email = 'sam@qq.com'**

create index 索引名 on 表名(列名1,列名2);

**索引名词**

#覆盖索引：在索引文件中直接获取数据
例如：
select name from userinfo where name = 'Sam50000';

#索引合并：把多个单列索引合并成使用
例如：
select * from  userinfo where name = 'Sam13131' and id = 13131;

**直接用索引字段查询，这种行为叫做覆盖索引**

**组合索引查询速度 > 索引合并查询速度**

**正确使用索引的情况**

**数据库表中添加索引后确实会让查询速度起飞，但前提必须是正确的使用索引来查询，如果以错误的方式使用，则即使建立索引也会不奏效。**

**使用索引，我们必须知道：**

**  （1）创建索引 **

**  （2）命中索引**

**  （3）正确使用索引**

**准备300w条数据：**

可参考:[使用pymysql--插入300万数据](http://www.py3study.com/Article/details/id/313.html)

**测试**

- like '%xx'
    select * from userinfo where name like '%zhang';
- 使用函数
    select * from userinfo where reverse(name) = 'zhangsan333';
- or
    select * from userinfo where id = 1 or email = 'zhangsan122@qq.com';
    特别的：当or条件中有未建立索引的列才失效，以下会走索引
            select * from userinfo where id = 1 or name = 'zhangsan1222';
            select * from userinfo where id = 1 or email = 'zhangsan122@qq.com' and name = 'zhangsan112';
- 类型不一致
    如果列是字符串类型，传入条件是必须用引号引起来，不然...
    select * from userinfo where name = 999;
- !=
    select count(*) from userinfo where name != 'zhangsan';
    特别的：如果是主键，则还是会走索引
        select count(*) from userinfo where id != 123;
- >
    select * from userinfo where name > 'zhangsan'
    特别的：如果是主键或索引是整数类型，则还是会走索引
        select * from userinfo where id > 123;
- order by
    select email from userinfo order by name desc;
    当根据索引排序时候，选择的映射如果不是索引，则不走索引
    特别的：如果对主键排序，则还是走索引：
        select * from userinfo order by id desc;

- 组合索引最左前缀
    如果组合索引为：(name,email)
    name and email       -- 使用索引
    name                 -- 使用索引
    email                -- 不使用索引

**尽量使用组合索引**

**什么是最左前缀呢？**

#最左前缀匹配：
#创建组合索引，name和email组合
create index ix_name_email on userinfo(name,email);
#执行下面3个sql

select * from userinfo where name = 'zhangsan';
select * from userinfo where name = 'zhangsan' and email='zhangsan@qq.com';
select * from userinfo where  email='zhangsan@qq.com';

name和email组合索引之后，查询：
（1）name        ---使用索引
（2）name和email ---使用索引
（3）email       ---不使用索引，因为没有name或者email字段
对于同时搜索n个条件时，组合索引的性能好于多个单列索引
******组合索引的性能>索引合并的性能*********

**索引的注意事项（重点）**

(1)避免使用select *
(2)count(1)或count(列) 代替count(*)
(3)创建表时尽量使用char代替varchar
(4)表的字段顺序固定长度的字段优先
(5)组合索引代替多个单列索引（经常使用多个条件查询时）
(6)尽量使用短索引 （create index ix_title on tb(title(16));特殊的数据类型 text类型）
(7)使用连接（join）来代替子查询
(8)连表时注意条件类型需一致
(9)索引散列（重复少）不适用于建索引，例如：性别不合适

**关于第7点，目前mysql5.7版本，没有区别。它和子查询速度是一样的。**

**关于第8点，假设有2个表，a和b。查询语句如下：**

select * from a left join b on b.pid=a.id

**务必保证on后面等式的字段类型是一致的**

**执行计划**

**explain + 查询SQL - 用于显示SQL执行信息参数，根据参考信息可以进行SQL优化**

mysql> explain select * from userinfo;
+----+-------------+----------+------+---------------+------+---------+------+---------+-------+
| id | select_type | table    | type | possible_keys | key  | key_len | ref  | rows    | Extra |
+----+-------------+----------+------+---------------+------+---------+------+---------+-------+
|  1 | SIMPLE      | userinfo | ALL  | NULL          | NULL | NULL    | NULL | 2973016 | NULL  |
+----+-------------+----------+------+---------------+------+---------+------+---------+-------+

mysql> explain select * from (select id,name from userinfo where id <20) as A;
+----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
| id | select_type | table      | type  | possible_keys | key     | key_len | ref  | rows | Extra       |
+----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
|  1 | PRIMARY     | <derived2> | ALL   | NULL          | NULL    | NULL    | NULL |   19 | NULL        |
|  2 | DERIVED     | userinfo   | range | PRIMARY       | PRIMARY | 4       | NULL |   19 | Using where |
+----+-------------+------------+-------+---------------+---------+---------+------+------+-------------+
rows in set (0.05 sec)

**参数说明:**

select_type：
查询类型
    SIMPLE          简单查询
    PRIMARY         最外层查询
    SUBQUERY        映射为子查询
    DERIVED         子查询
    UNION           联合
    UNION RESULT    使用联合的结果

table：
    正在访问的表名
type：
    查询时的访问方式，性能：all < index < range < index_merge < ref_or_null < ref < eq_ref < system/const
    ALL 全表扫描，对于数据表从头到尾找一遍
    select * from userinfo;
    特别的：如果有limit限制，则找到之后就不在继续向下扫描
       select * from userinfo where email = 'zhangsan112@qq.com'
       select * from userinfo where email = 'zhangsan112@qq.com' limit 1;
       虽然上述两个语句都会进行全表扫描，第二句使用了limit，则找到一个后就不再继续扫描。

INDEX ： 全索引扫描，对索引从头到尾找一遍
    select nid from userinfo;

RANGE： 对索引列进行范围查找
    select *  from userinfo where name < 'zhangsan';
    PS:
        between and
        in
        >   >=  <   <=  操作
        注意：!= 和 > 符号

INDEX_MERGE： 合并索引，使用多个单列索引搜索
    select *  from userinfo where name = 'zhangsan' or nid in (11,22,33);

REF： 根据索引查找一个或多个值
    select *  from userinfo where name = 'zhangsan112';

EQ_REF： 连接时使用primary key 或 unique类型
    select userinfo2.id,userinfo.name from userinfo2 left join tuserinfo on userinfo2.id = userinfo.id;

CONST：常量
    表最多有一个匹配行,因为仅有一行,在这行的列值可被优化器剩余部分认为是常数,const表很快,因为它们只读取一次。
    select id from userinfo where id = 2 ;

SYSTEM：系统
    表仅有一行(=系统表)。这是const联接类型的一个特例。
    select * from (select id from userinfo where id = 1) as A;

possible_keys：可能使用的索引

key：真实使用的

key_len： MySQL中使用索引字节长度

rows： mysql估计为了找到所需的行而要读取的行数 ------ 只是预估值

extra：
    该列包含MySQL解决查询的详细信息
    "Using index"
        此值表示mysql将使用覆盖索引，以避免访问表。不要把覆盖索引和index访问类型弄混了。
    "Using where"
        这意味着mysql服务器将在存储引擎检索行后再进行过滤，许多where条件里涉及索引中的列，当（并且如果）它读
        取索引时，就能被存储引擎检验，因此不是所有带where子句的查询都会显示"Using where"。有时"Using where"的
        出现就是一个暗示：查询可受益于不同的索引。
    "Using temporary"
        这意味着mysql在对查询结果排序时会使用一个临时表。
    "Using filesort"
        这意味着mysql会对结果使用一个外部索引排序，而不是按索引次序从表里读取行。mysql有两种文件排序算法，这
        两种排序方式都可以在内存或者磁盘上完成，explain不会告诉你mysql将使用哪一种文件排序，也不会告诉你排序
        会在内存里还是磁盘上完成。
    "Range checked for each record(index map: N)"
        这个意味着没有好用的索引，新的索引将在联接的每一行上重新估算，N是显示在possible_keys列中索引的位图，
        并且是冗余的

**重点：**

**查询时的访问方式，性能：all<index<range<index_merge<ref_or_null<ref<eq_ref<system/const**

**尽量使用主键索引，它的查询速度是最快的。**

**预估sql语句的查询性能**

**mysql慢日志记录**

**开启慢查询日志，可以让MySQL记录下查询超过指定时间的语句，通过定位分析性能的瓶颈，才能更好的优化数据库系统的性能。**

(1) 进入MySql 查询是否开了慢查询
    show variables like 'slow_query%';
    参数解释：
     slow_query_log 慢查询开启状态  OFF 未开启 ON 为开启
    slow_query_log_file 慢查询日志存放的位置（这个目录需要MySQL的运行帐号的可写权限，一般设置为MySQL的数据存放目录）

（2）查看慢查询超时时间
    show variables like 'long%';
    ong_query_time 查询超过多少秒才记录   默认10秒 

（3）开启慢日志（1）（是否开启慢查询日志，1表示开启，0表示关闭。）
    set global slow_query_log=1;
（4）再次查看
    show variables like '%slow_query_log%';

（5）开启慢日志（2）：（推荐）
    在my.cnf 文件中
    找到[mysqld]下面添加：
    slow_query_log =1
    slow_query_log_file=C:\mysql-5.6.40-winx64\data\localhost-slow.log
    long_query_time = 1

    参数说明：
    slow_query_log 慢查询开启状态  1 为开启
    slow_query_log_file 慢查询日志存放的位置
    long_query_time 查询超过多少秒才记录   默认10秒 修改为1秒

**修改配置文件之后，需要重启mysql服务**

**执行一个超过1秒的sql，查看慢日志文件**

#执行慢sql，超过1秒的
mysql> select * from userinfo where name = 999;
Empty set, 65535 warnings (1.77 sec)

#查看慢日志文件路径
mysql> show variables like '%slow_query_log%';
+---------------------+--------------------------------------------------------------------------+
| Variable_name       | Value                                                                    |
+---------------------+--------------------------------------------------------------------------+
| slow_query_log      | ON                                                                       |
| slow_query_log_file | D:\Program Files (x86)\mysql-5.7.22-winx64\data\DESKTOP-CFMVJ8G-slow.log |
+---------------------+--------------------------------------------------------------------------+
2 rows in set, 1 warning (0.00 sec)

#打开文件DESKTOP-CFMVJ8G-slow.log，内容如下：

MySQL, Version: 5.7.22 (MySQL Community Server (GPL)). started with:
TCP Port: 3306, Named Pipe: MySQL
Time                 Id Command    Argument
MySQL, Version: 5.7.22-log (MySQL Community Server (GPL)). started with:
TCP Port: 3306, Named Pipe: (null)
Time                 Id Command    Argument
# Time: 2018-06-19T12:19:53.239515Z
# User@Host: root[root] @ localhost [::1]  Id:     2
# Query_time: 1.767427  Lock_time: 0.003748 Rows_sent: 0  Rows_examined: 3000000
use db1;
SET timestamp=1529410793;
select * from userinfo where name = 999;

可以看到Query_time的时间为1.767427秒

**分页性能相关方案**

第1页：
select * from userinfo limit 0,10;
第2页：
select * from userinfo limit 10,10;
第3页：
select * from userinfo limit 20,10;
第4页：
select * from userinfo limit 30,10;
......
第2000010页
select * from userinfo limit 2000000,10;

PS:会发现，越往后查询，需要的时间约长，是因为越往后查，全文扫描查询，会去数据表中扫描查询

**最优的解决方案**

** (1) 只有上一页和下一页**

**语法:**

下一页：
select * from userinfo where id>max_id limit 10;

上一页：
select * from userinfo where id<min_id order by id desc limit 10;

**因为使用where id<min_id，默认是从1开始的。但是min_id是一个中间值，所以需要order by id desc，才能得到想要的id，最后使用limit取出指定的长度，就是最终的结果**

**(2) 中间有页码的情况**

****语法：****

select * from (select * from userinfo where id > pre_max_id limit (cur_max_id-pre_max_id))*10) as A order
 by id desc limit 10;