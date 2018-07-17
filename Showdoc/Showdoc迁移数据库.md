Showdoc迁移数据库
===

## 迁移数据库

### 为什么要迁移

ShowDoc在很早期使用Mysql来存储数据，但后来决定转用Sqlite数据库。如果你是早期使用mysql那部分的用户，可以考虑迁移数据库到Sqlite，否则以后无法再使用官方新增的任何功能。ShowDoc使用Sqlite的理由如下：

*   PHP环境默认支持Sqlite，所以只需要安装好PHP环境，即可使用ShowDoc，无须再安装mysql。这对于不懂mysql的开发者（如App开发者）来说会更容易一些。同时方便官方维护ShowDoc，不用维护和测试两个数据库版本

*   Sqlite的性能并不差，对于总项目数在一万以内的情况，基本不用考虑性能问题。所以完全足够普通公司或者团队的使用。

*   Sqlite数据库文件放在/Sqlite目录下，迁移和备份都十分简单，直接复制/转移该目录即可

### 如何迁移

*   备份所有代码和数据库

*   下载新代码：[https://github.com/star7th/showdoc](https://github.com/star7th/showdoc)

*   将新代码中的/Sqlite/ 复制到旧目录(如果已存在/Sqlite则覆盖之)，并赋予/Sqlite/showdoc.db.php可写权限

*   复制新代码中的/Application/Home/Controller/UpdateController.class.php 覆盖原来旧的相应文件。

*   在浏览器访问：[http://xxxx.com/index.php?s=/home/update/toSqlite](http://xxxx.com/index.php?s=/home/update/toSqlite) ，看到ok提示后，mysql的数据已经写入/Sqlite/showdoc.db.php

*   此时，除了/Sqlite/showdoc.db.php文件外，旧目录的其他所有文件全部用新下载的文件覆盖。注意清理runtime缓存以及保留原来文件夹权限的设定。具体哪些文件需要权限，可参考部署手册。
