# Linux安装多个Python版本
@toc

因为环境或学习的需要，我们可能需要在服务器上安装多个python版本，安装一个更新的python版本后，使用软链来进行共存。

这个时候需要进行源码编译安装。

当然后续开发项目可以直接跑在虚拟环境下隔离开来，就不再需要在服务器上安装多个不同版本的包了。

## 下载Python源码

从[http://www.python.org/download/](http://www.python.org/download/)根据需要的版本下载源文件。

![python-tar]($resource/python-tar.jpg)
例如上图就是我在官网直接找到3.5.6版本的下载页面，点击的tar源码包进行下载。

## 编译安装

### 补充
注意编译安装python前最好先保证系统己安装上以下库
```
yum install zlib
yum install zlib-devel
yum install openssl
yum install openssl-devel
yum install readline-devel
```

**解压源码包**
`tar zxvf Python-3.5.6.tgz`
`cd Python-3.5.6`
**配置选项**
`sudo ./configure --enable-optimizations --prefix=/usr/local/python-3.5.6 --with-zlib --with-readline`
`--enable-optimizations` 为最优安装，建议使用这个参数。
`--prefix`为指定安装的路径

进行编译安装
```
sudo make
sudo make install
```

## 修改Python软链 

默认python命令是在/usr/bin/目录下，需要在这里把软链修改成3.5.6的版本，顺便建立一个3.5.6的软链。

![python-ln]($resource/python-ln.jpg)
上图是原默认软链接，是python2.7的，其实它也己经默认建立了一个python2的软链接了。

重新命名默认python软链接为python2.7以便3.5的版本成为默认环境变量
`sudo mv /usr/bin/python /usr/bin/python2.7`
建立python3.5版本的软链接
`sudo ln -s /usr/local/python-3.5.6/bin/python3.5 /usr/bin/python`

当然，也可以不做上一步，而是将3.5版本的软链接成python3之类也是可行的。
![python3-ln]($resource/python3-ln.jpg)
比如我做的就是新增python3的软链接指向python3.5所在的目录。
顺便也将配置文件也做了一个软链接指向：
`sudo ln -s /usr/local/python-3.5.6/bin/python3.5-config /usr/bin/python3-config`

### 补充
**注意** 如无特别需要，推荐命名成不同python的软链接，因为你会发觉要是你直接改了原来指向旧版本的软链接的话，pip，virtualenv等依赖原版本python的工具需要改它的py文件头的环境变量。

![python-pip-bin]($resource/python-pip-bin.jpg)

如果你修改了原软链接，使得python实际指向的是新版本的python的话，pip等仍依赖旧版本的工具要么重装要么修改它py文件的首行，按实际情况重新指向你的旧版本原版本的python环境，例如此处我就可直接改成`/usr/bin/python2`

## 虚拟环境设置不同版本的python

创建好项目目录，cd进到目录下。

在创建python虚拟环境时，可以指定虚拟环境要使用的python版本，例如下命令（使用-p参数指明python解释器的路径）：

```bash
  -p PYTHON_EXE, --python=PYTHON_EXE
                        The Python interpreter to use, e.g.,
                        --python=python3.5 will use the python3.5 interpreter
                        to create the new environment.  The default is the
                        interpreter that virtualenv was installed with
                        (/usr/bin/python)

```
例：
```bash
virtualenv -p /usr/bin/python2 ENV2.7  #创建python2.7的虚拟环境
virtualenv -p /usr/bin/python3 ENV3.5  #创建python3.5的虚拟环境
virtualenv --python=python3 my-env     #创建python3.5的虚拟环境
```

之所以装多个版本的python，有一个原因很重要，也就是想要使用virtualenv创建隔离的虚拟环境的时候指定不同python版本。但是前面我进行编译安装的时候没有指定编译安装上zlib库，就会出现以下如图问题：

![virtualenv-python3]($resource/virtualenv-python3.jpg)
报找不到zlib模块错误。

所以最好还是系统原环境变量安装成高版本的python，或进行python编译安装的时候记得加上`--with-zlib`。
最好`--with-zlib-devel`和`readline`也带上。

当然，补救办法也是有的，就是重新进行编译安装。
(重新进行本文最开始的操作，当然本文最开始笔者己经补充更正过了编译安装时的配置。）

笔者由于重新编译后仍然报zlib模块错误，笔者并不想弄污原来python2.7的环境，所以决定尝试virtualenv下载源码来使用，而zlib也进行编译安装来让python3也能正常导入zlib模块。

### 下载zlib模块并进行编译安装
官网[http://www.zlib.net/](http://www.zlib.net/)下载最新版本的zlib源码文件，我下载的是`zlib-1.2.11.tar.gz`

安装zlib:
`tar xzvf zlib-1.2.11.tar.gz`
`cd zlib-1.2.11`

建议默认路径安装，编译三部曲：
```bash
./configure
make
make install
```

### python导入zlib
如下图，编译安装完成zlib后，进python3测试己经可能正常导入zlib包。
![python3-zlib]($resource/python3-zlib.jpg)

zlib安装完后，`libz.a`在`/usr/local/lib/,opensuse`中`zlib.h`默认放在`/usr/local/include/`中。
其它linux的`zlib.h`文件一般在`/usr/include`中。

### 重新编译python并指定zlib
如果还发生少数导入不成功，或你想直接软链接升级python旧版本的，可能需要重新编译python。

进入Python源码文件目录，重新编译Python

`sudo ./configure --enable-optimizations --prefix=/usr/local/python-3.5.6 --with-zlib=/usr/include`
或者：
`./configure --enable-optimizations --prefix=/usr/local/python-3.5.6 --with-zlib-dir=/usr/local/lib`

都可以完成python对zlib库的支持，在python源码中直接import zlib即可使用zlib了。

### 下载virtualenv源码包
直接到[pypi](https://pypi.org/project/virtualenv/#files)下载16.0.0版本。
tar命令进行解压
`tar xvfz virtualenv-16.0.0.tar.gz`
`cd virtualenv-16.0.0`
此目录下文件如下，我们主要用到的并不是setup安装，而是直接使用它的`virtualenv.py`文件。

![virtualenv-python3-run]($resource/virtualenv-python3-run.jpg)

## virtualenv创建虚拟环境
在项目目录下直接指定python版本及virtualenv.py的绝对路径进行创建虚拟环境：
 `sudo python3 /home/operation/virtualenv-16.0.0/virtualenv.py py3`

![python3-virtualenv-py]($resource/python3-virtualenv-py.jpg)

可以不进行全局安装，而是用户下进行虚拟化环境可以直接使用python3 后接`virtualenv.py`运行，完美解决需求。

也可以使用原python2下的全局virtualenv来执行，一样可以创建，如：
```
operation@opensuse-wordpress:/work> sudo virtualenv --python=python3 my_py3_env
[sudo] password for root: 
Running virtualenv with interpreter /usr/bin/python3
Using base prefix '/usr/local/python-3.5.6'
New python executable in /work/my_py3_env/bin/python3
Also creating executable in /work/my_py3_env/bin/python
Please make sure you remove any previous custom paths from your /root/.pydistutils.cfg file.
Installing setuptools, pip, wheel...done.
```

虚拟隔离环境目录如下：
![py3-virtualenv]($resource/py3-virtualenv.jpg)

具体virtualenv的使用命令等见相关文章，在此就不再详述。
如此，在liunx下己经可以实现多版本的python完美共存。

2018-8-15 铁乐与猫
end

