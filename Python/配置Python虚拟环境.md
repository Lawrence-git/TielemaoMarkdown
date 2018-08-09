# 配置Python虚拟环境

@toc

# opensuse/linux系统的python配置virtualenv

## pip升级
python有一个叫做PyPI（http://pypi.python.org/pypi)的公共资源库，它管理着python相关的各个功能包。
类似nmp，rpm一样。
我们可以直接利用PyPI来下载安装一些依赖包。
为此，先将pip升级一下：
`sudo pip install --upgrade pip`
确认版本号
`pip --version` 
查看当前环境下己经安装好的所有包版本信息
`pip freeze`

## 安装virtualenv
virtualenv是虚拟不同的Python运行环境的工具包。
它就像虚拟机一样是属于局部的，并不影响在全局（实际系统上）的环境。
* 对多种不同的python运行环境进行管理
* 起到相互隔离的作用

在日常Python项目开发中，比如除了基于Flask的项目外，还会有例如Django的项目用到Python。
当项目越来越多时就会面对使用不同版本的Python的问题，或者至少会遇到使用不同版本的Python库的问题。
出现的问题会有：
* 库常常不能向后兼容，更不幸的是任何成熟的应用都不是零依赖。
* 如果两个项目依赖出现冲突，就会比较麻烦。

 而Virtualenv就可以用来解决Python多版本环境的问题。
 它的基本原理是：
 * 为每个项目安装一套Python，多套Python并存。
 * 但它不是真正地安装多套独立的Python拷贝，
 * 而是使用了一种巧妙的方法让不同的项目处于各自独立的环境中。
 
`sudo pip install virtualenv`或`easy_install virtualenv`
Ubuntu系统则是`apt-get install python-virtualenv`

同样，查看版本号
`virtualenv --version`
查看帮助
`virtualenv --help`

## 使用virtualenv建立虚拟运行环境 

* 建立项目目录，例如命名为work：
`mkdir ~/work`
* 进入项目目录
`cd ~/work`
* 创建虚拟环境，比如命名为my-env
`virtualenv my-env`
```bin
operation@opensuse-wordpress:~/work> virtualenv my-env
New python executable in /home/operation/work/my-env/bin/python
Installing setuptools, pip, wheel...done.
```
* 激活my-env Python虚拟环境，并进入该虚拟环境
`source my-env/bin/activate`
终端提示符会有提示，仍可以切换到任何目录中执行，而不局限于在my-env目录中;
```bin
operation@opensuse-wordpress:~/work> source my-env/bin/activate
(my-env) operation@opensuse-wordpress:~/work> 
```

如果是windows平台的话，一般为直接到脚本执行目录下执行
例：
```
# 在win平台下做的虚拟环境，目录路径会有所不同，直接去到目录下执行activate即可
env\Scripts>activate
```

* 检查虚拟环境中安装好的包版本信息
`pip freeze`
* 退出my-env Python虚拟环境虚拟环境
`deactivate`
注意：
  * 此时终端提示符会变回不带虚拟环境命名前缀的正常提示符
  * virtualenv虚拟环境退出后，在虚拟环境中启动的服务进程，并不会退出。

* virtualenv默认设置创建干净的虚拟运行环境
    `virtualenv --no-site-packages my-env`
  * 当要使用系统下的模块或者包来创建虚拟运行环境时，命令如下：
    `virtualenv --system-site-packages my-env`
 
在安装了最少数量包的干净python虚拟环境中，我们可以使用pip来进行其它项目所需的包和模块，
这样一来就方便掌控和把握并尽可能减少其他不相干的包或模块给应用程序开发带来的未知影响和干扰。

* 当使用virtualenv建立虚拟运行环境来开发项目时，千万不要忘了先激活虚拟运行环境。
* 而当开发完成，不再需要该虚拟环境时，直接使用rm等命令将对应的目录删除即可。

* 指定虚拟环境使用的python版本
在创建python虚拟环境时，可以指定虚拟环境要使用的python版本，例如下命令（使用-p参数指明python解释器的路径）：

```python
virtualenv -p /usr/bin/python2.7 ENV2.7  #创建python2.7的虚拟环境virtualenv -p /usr/bin/python3.4 ENV3.4  #创建python3.4的虚拟环境
```

# virtualenvwrapper 统一管理虚拟环境
virtualenvwrapper在virtualenv基础上进行了二次封装，
便于统一对所有的虚拟环境进行管理。

解如诸如：
* virtualenv需要每次使用source命令导入虚拟运行环境信息
* 开发者还可能忘记虚拟环境目录的建立位置等

## 安装virtualenvwrapper
`pip install virtualenvwrapper`
安装完成后可以先搜索一下virtualenvwrapper.sh这个脚本，不同版本有放在`/usr/local/bin`下的，有放在`/usr/bin`下的。
```
operation@opensuse-wordpress:/usr/local/bin> locate virtualenvwrapper.sh
/usr/bin/virtualenvwrapper.sh
```
# Mact系统的python配置virtualenv

## 1.安装virtualenv

`pip3 install virtualenv` 

## 2.创建项目目录

```bash
mkdir Myproject
cd Myproject 
```

## 3.创建独立运行环境-命名

```
virtualenv --no-site-packages venv
#得到独立第三方包的环境
```


## 4.进入虚拟环境

```
source venv/bin/activate
#此时进入虚拟环境(venv)Myproject 
```

## 5.安装第三方包

```
(venv)Myproject: pip install django==1.9.8 
#此时pip的包都会安装到venv环境下，venv是针对Myproject创建的 
```


## 6.退出venv环境

`deactivate命令 `

## 7.virtualenv是如何创建“独立”的Python运行环境的呢？

原理很简单，就是把系统Python复制一份到virtualenv的环境，
用命令`source venv/bin/activate`进入一个virtualenv环境时，virtualenv会修改相关环境变量，让命令python和pip均指向当前的virtualenv环境。