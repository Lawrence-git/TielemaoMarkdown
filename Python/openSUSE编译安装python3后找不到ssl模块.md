# openSUSE编译安装python3后找不到ssl模块

这会导致你在配置虚拟环境后使用pip去下载包的时候报https连接的错误！

所以我在碰到这个情况并千辛万苦解决后将此步骤放到这里作为重要补充，以免你像我那样多次重新编译（极其耗时！）

下图是我碰到的在虚拟环境python3.5.6版本下，使用pip安装django时报的错，就是因为编译安装时没有指定ssl。

![](https://www.tielemao.com/wp-content/uploads/2018/08/pip-in-django-ssl.jpg)

**解决办法**

由于我使用的是openSUSE的系统，所以我使用`yzpper in openssl openssl-devel`安装好ssl了，但仍需要重新编译安装才能正确导入ssl模块。

在这里最好直接编辑python3.5.6的源安装配置文件，毕竟openSUSE的ssl默认并不是装在`/usr/local/ssl`。

* 使用vim进行编辑：

`vim ~/Python3.5.6/Modules/Setup.dist`

* 未编辑之前是长这样的，有关ssl的配置集中在207-210四行中。

![](https://www.tielemao.com/wp-content/uploads/2018/08/python3-setup-ssl.jpg)

但我用的是openSUSE系统，所以只需要编辑208-210三行，也就是`#SSL=/usr/local/ssl`不要去解除注释，以免弄巧成拙，当然我感觉要设置成我系统的`/etc/ssl`可能也是可以的。

* 编辑，也就是去掉相关行注释之后如下图：

![](https://www.tielemao.com/wp-content/uploads/2018/08/opensuse-python3-ssl.jpg)

之后再进行下一步的编译三步。

**配置选项**

`sudo ./configure --enable-optimizations --prefix=/usr/local/python-3.5.6`

`--enable-optimizations` 为优化性能的选项，建议使用上这个参数。

`--prefix`为指定安装的路径

注：python3.5.6我多次尝试过了，它己经不认`--with-ssl`，`with-zlib`等选项了，我感觉这可能是一种进步，因为我如果漏装了zlib和readline的话，系统重新安装上就是了，不用再重新编译python3也能正确导入了。但注意的是ssl视系统情况不同而不同，openSUSE下ssl的话还是得重新编译且是在`setup.dist`中配置。

进行编译安装

```
sudo make
sudo make install
```

编译安装完成后，可以运行python3解释器后，敲入`import ssl`命令。
如没有报`no model named`错误，就表示可以正常使用ssl了。

2018-8-15
by 铁乐与猫

【end】

