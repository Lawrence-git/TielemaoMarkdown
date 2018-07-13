[[toc]]

## 引子
mysql官方网站上没有 windows mysql5.7 64位版本msi的安装包下载，我们可以通过zip版本解压缩后手动安装配置环境。
msi安装的话有32位的，基本上就是看着图形界面来一步步操作，没有技术含量，不在此阐述。

另外截至2018年6月份，mysql 8.0.11版本己经发布，不过大部分公司目前还是在使用mysql5.x版本的吧。有兴趣的话可以提前学习一下8.0.11版本的。

不过最推荐的还是mysql被oracle收购后，使用其分支MariaDB。

## 环境和注意事项

* win7及以上操作系统
* MySQL5.7.22 zip格式安装包
* 5.7版本和之前的不一样：
	* 文件夹中没有DATA目录
	* 没有mysql默认库
	* 没有默认的my.ini或参考的my-default.ini
	* 那是因为它的初始化方法和之前的不一样了
* Windows的路径使用了反斜杠（\），因此，配置中使用时尽量合乎规范将反斜杠改为双反斜杠(\\)或直接使用斜杠（/）。（也有直接使用\而不受影响的）

## 下载

* 下载mysql5.7.22 zip安装包
	* 官网下载压缩包链接： 		https://dev.mysql.com/downloads/mysql/5.7.html#downloads

* 根据电脑配置选择32/64位版本

* 将下载回来的mysql压缩包解压至适当路径，也就是你打算以后使用的工作目录

## 配置环境变量

* 配置环境变量：控制面板->系统和安全->系统->高级系统设置-环境变量-找到Path变量-点击编辑

* 如图，将你自己所定的工作目录，包含bin的路径填进去，例如我的就是`E:\mysql\bin`,注意和其它变量值以英文的分号`;`分隔开。
![](https://www.tielemao.com/wp-content/uploads/2018/06/mysql_Path.jpg)

* 配置好环境变量是为了方便我们不用每次都进入到mysql的目录下执行。接下来是配置my.ini文件，是为了一些自定义设置。
下载回来的根目录下没有my-default.ini 文件，需要直接在mysql文件夹下新建文本文档，重命名为my.ini，my.ini配置内容如下：

```
[mysqld]
# 服务端配置

# 设置mysql的工作目录，安装包解压后的路径
basedir=E:\\mysql

# 数据存放目录data，需要自行新建
# 也可以使用mysqld --initialize-insecure 命令后也会自动在根目录中生成data目录
datadir=E:\\mysql\data

# 默认连接端口3306，正式环境一般都会修改
port=3306

# 设置mysql默认字符集为utf-8
character-set-server=utf8

[client]
# 客户端配置

default-character-set=utf8
#设置mysql默认字符集为utf-8
```
## 初始化mysql
* 在mysql根目录下新建data空文件夹。（也可不见而等初始化命令自动建）

* 以管理员身份运行cmd命令

* mysql初始化
	* 输入cd 对应mysql\bin目录,例如我的是E:\mysql\bin，进入bin目录下
	* 输入mysqld --initialize（初始化）
	* 或mysqld --initialize-insecure（不安全的选项）	 
	* 两者取决于你是否希望服务器生成一个拥有随机初始密码的`root@localhost`的账户。后者直接是空密码创建。
	* 为了能够方便查看初始化过程中的信息，可以追加 `--console` 参数使mysqld将输出信息写到控制台。
	* 一般linux系统才需要追加`--user=mysql`之类来指定用户（事先设好读写权限）
	* 例，如图：
	![](https://www.tielemao.com/wp-content/uploads/2018/06/mysqld_initialize.jpg)
	* 记下最后一行产生的随机密码。

## 安装（到windwos）服务
* 同样在管理员权限的cmd中操作，安装成服务更便利开机启动。
* 输入`mysqld --install`
* 成功会如图显示`Servers Successfully installed`
![](https://www.tielemao.com/wp-content/uploads/2018/06/mysql_install_start.jpg)
	* 若需要指定配置文件（mysql多实例的）则可以在`--install`后面跟自定义的服务名和`--defaults-file`选项来指定配置文件。
`mysqld  --install MySQL --defaults-file=E:\mysql\my.ini`
上述命令可以在安装时指定服务名为 `MySQL` 以及指定配置文件路径，需要注意的是：`--install`必须是第一个参数， 且服务名（若需指定的话）必须紧跟其后。
	
	* 如果不想让MySQL服务每次开机都自动启动，可以使用`--install-manual` 参数代替 `--install` 参数。

* 控制台下输入`net start mysql` 启动mysql服务。
* 在windows的控制面板，服务界面下也能查看到mysql服务。
![](https://www.tielemao.com/wp-content/uploads/2018/06/mysqld_server.jpg)
   * 卸载mysql服务使用的命令是`mysqld --remove`
   * 正常退出和关闭mysql服务使用`net stop mysql`
![](https://www.tielemao.com/wp-content/uploads/2018/06/mysql_stop.jpg)
   * 也有使用mysqladmin自带的管理工具来关闭的，前提是进入到bin目录下运行：`mysqladmin -u root -p shutdown`
例如我的是`E:\mysql\bin>mysqladmin -u root -p shutdown`

* linux中我们有命令来查看进程号和杀死进程，windows中也有，查看相关进程号使用的是`tasklist | findstr mysql`
   * 杀死进程（不推荐）的是`taskkill /F /PID 进程号`
![](https://www.tielemao.com/wp-content/uploads/2018/06/taskkill_mysql.jpg)

* 错误日志：如果 mysqld 没能启动成功，则可以查看 `error log` 文件，该文件在配置文件中指定的 datadir 目录中，后缀名`.err` 。`error log` 文件是可以通过 `--log-error` 参数指定的，另外，如果想让 mysqld 将错误日志输出到控制台，可以使用 `--console` 参数。

## 登录和重设密码

* 启动mysql服务，登录连接到mysql服务器。
	* `mysql -uroot -p`输入前面记下的随机密码登入。
	![](https://www.tielemao.com/wp-content/uploads/2018/06/mysql_uroot_p.jpg)
	* 若之前你使用`--initialize-insecure` 参数初始化，则使用如下命令来连接MySQL：| 
`mysql  -uroot --skip-password`或同样使用`mysql -uroot -p`在提示输入密码时直接回车即可进入。
* mysql5.7强制你无论做何操作都要先将随机密码重设成自己定义的密码。
* 一般第一次可用mysqladmin方式重设root密码，如图：
`mysqladmin -uroot -p password`
![](https://www.tielemao.com/wp-content/uploads/2018/06/mysqladmin-password.jpg)

* 重新正常登录数据库后，也可直接在mysql内直接修改用户权限或user表方式修改密码。
	* 例：
`mysql> update mysql.user set authentication_string=password('') where User="root
" and host="localhost";`
	* 5.7以前的版本是使用`password`字段保存密码的，5.7改成了`authentication_string`，不容易记忆。
	* 修改密码后需要输入`flush privileges;`命令来刷新生效。
	* 由于`authentication_string`不太容易记忆，也有直接改权限的，例：
	`ALTER USER 'root'@'localhost' IDENTIFIED BY '123456'`
	![](https://www.tielemao.com/wp-content/uploads/2018/06/mysql57-password01.jpg)
	* 同样`flush privileges;`命令来刷新权限即生效。

## 强制跳过密码登录

* 适用于忘记密码或个人学习使用并不想每次连接输入密码，但后者可以设置密码为空，所以此情景主要还是用在忘记密码了，需要跳过密码来登录后重设密码的情景。

* 第一种，临时跳过密码。
	* 执行 `mysqld --skip_grant_tables` 启动服务
	* 注意此时应再开多一个cmd窗口来作为客户端连接服务端，登录的时候直接回车无需密码。
	* 然后就是重设密码了。设置成功后记得停止mysqld服务，重新启动正常需密码的服务。

* 第二种，需长期跳过密码（所有用户都不用密码即可连接）。
	* 在配置文件，my.ini中的`[mysqld]`下添加一行
	`skip_grant_tables`表示跳过权限表。
	* 再执行mysql服务启动，就是无权限管理的连接了。极不安全，只适用于个人测试或学习环境。
	* 此配置一成功后，客户端连接mysql只需敲mysql就直接进mysql了。如图：
	![](https://www.tielemao.com/wp-content/uploads/2018/06/mysql_skip_grant_tables.jpg)

* 注意，skip_grant_tables 中间间隔是下划线。

## 设置友好提示符

连接上去，使用的时候，你会发觉MySQL 客户端的默认提示符是 "mysql>"，基本上没什么实际作用。
修改这个提示符，让它显示一些有用的信息，例如当前所在的数据库等。
修改方法有四种，其中前两种只对当前连接有效，后两种则对所有连接有效。
 
* 1、连接客户端时通过参数指定。
`mysql --prompt="(\u@\h) [\d]> "  `
这样提示符就会变成 `(user@host) [database]>`
其中常用的字符参数有：

>\D  完整的日期
\d  当前数据库
\h  服务器地址
\u  用户名

* 2、连接上客户端后，通过 prompt命令 `PROMPT (\u@\h) [\d]>  `修改。
例：
```
mysql> PROMPT (\u@\h)[\d]>
PROMPT set to '(\u@\h)[\d]>'
```
* 3、在 MySQL 的配置文件中配置。
```
[mysql]  
 prompt=\\u@\\h [\\d]>\\
```
* 4、通过环境变量配置。
`export MYSQL_PS1="\u@\h [\d]> "  `

【end】

