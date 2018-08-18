# Python生态工具

## 下载服务器

python内置了一个下载服务器。
进入到需要共享出去的目录下，执行下面命令：
`python -m http.server`

python2的话则是
`python -m SimpleHTTPServer`

![python-httpserver]($resource/python-httpserver.jpg)

简单的开启了一个web共享服务器，可以提供下载。例如我点击页面上的tar包就会出现下载框。
注意的是如果点击py文件之类是直接打开。。
可以在这个基础上导入模块再写一个http服务器。

## 检查第三方库是否正确安装

安装完python第三方库后，可以运行python解释器，尝试进行import导入。
如果没有报错，则是安装成功。
如果是使用脚本对大批量的服务器进行自动部署，更方便的验证是使用以下命令：
`python -c "import 模块名"`

![python-c]($resource/python-c.jpg)
这样可以省去交互式验证，就能够在脚本中实现对于远程服务器的验证操作。

## pip常用命令

pip的子命令

|子命令|解释说明|
|-|-|
|install|安装软件包|
|download|下载安装包|
|uninstall|卸载安装包|
|freeze|按照requirements格式输出安装包，可以到其他服务器上执行pip install -r requirements.txt 直接安装软件|
|list|列出当前系统中的安装包|
|show|查看安装包的信息，包括版本、依赖、许可证、作者、主页等信息|
|check|检查安装包的依赖是否完整|
|search|查找安装包|
|wheel|打包软件到whell格式|
|hash|计算安装包的hash值|
|completion|生成命令补全配置|
|help|获取pip和子命令的帮助信息|

例：
* 导出系统己安装的安装包列表到`requirements`文件
`pip freeze > requirements.txt`
类似于npm的`package.json`

* 从requirements文件安装:
`pip install -r requirements.txt`

* 使用pip命令补全：
```bash
pip completion --bash >> ~/.profile
source ~/.profile
```

* 使用命令补全以后，通过键入`pip i<tab>`，将会自动输入`pip install`。

### 加速pip安装的技巧

* 使用豆瓣或阿里云的源加速
  * 直接指定镜像源的地址：
`pip install -i https://pypi.douban.com/simple/ flask`
  * 配置镜像源：
    修改pip配置文件，将镜像源写入配置文件中。
    对于Linux系统来说，需要创建`~/.pip/pip.conf`文件，然后写入：
```
[global]
index-url = https://pypi.douban.com/simple/
```

* 将软件下载到本地部署
对于使用脚本部署大量的服务器非常有用，而且对于无法连接外网的服务器也可以使用这种方法：

```
# 下载到本地
pip install --download='pwd' -r requirments.txt

# 本地安装
pip install --no-index -f file://'pwd' -r requirments.txt
```
使用这种方式，只需要下载一次，就可以多次安装。并且，pip能够自动处理软件依赖。下载一个包，实际上会连那个包所需的依赖包也会同样下载到本地上。

参考
《Python Linux系统管理与自动化运维》第2章