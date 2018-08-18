# CentOS6源码安装vim8

vim8相比vim7多了很多功能。
不过需要源码来进行安装。

* 移除旧版本的vim
`yum remove vim`

* 安装依赖库
```
sudo yum install -y ruby ruby-devel lua lua-devel luajit \
luajit-devel ctags git python python-devel \
python3 python3-devel tcl-devel \
perl perl-devel perl-ExtUtils-ParseXS \
perl-ExtUtils-XSpp perl-ExtUtils-CBuilder \
perl-ExtUtils-Embed libX11-devel
```
还有一个比较旧的库，单独拿出来说
`yum install ncurses-devel -y`

* 去github克隆源代码回来
`git clone https://github.com/vim/vim.git`

* 进入源码目录
`cd vim/src`

* 编译并添加python支持，注，下面的参数可选
```
./configure --with-features=huge --enable-python3interp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ --enable-rubyinterp --enable-luainterp --enable-perlinterp --with-python3-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu/ --enable-multibyte --enable-cscope --prefix=/usr/local/vim/

make
make install
```

参数说明如下：
```
--with-features=huge：支持最大特性
--enable-rubyinterp：打开对ruby编写的插件的支持
--enable-pythoninterp：打开对python编写的插件的支持
--enable-python3interp：打开对python3编写的插件的支持
--enable-luainterp：打开对lua编写的插件的支持
--enable-perlinterp：打开对perl编写的插件的支持
--enable-multibyte：打开多字节支持，可以在Vim中输入中文
--enable-cscope：打开对cscope的支持
--with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ 指定python 路径
--with-python-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu/ 指定python3路径

--prefix=/usr/local/vim：指定将要安装到的路径(自行创建)
```

* 导入环境变量
注意此时编译完成后，直接运行vim是找不到的。
还需要配置一下环境变量。

编辑文件：
`/usr/local/bin/vim /etc/profile.d/path.sh`输入内容：

```
#!/bin/bash
export PATH=$PATH:/usr/local/bin/vim
```
导入环境变量
`source /etc/profile.d/path.sh`
这个时候就可以敲vim如常运行了。

![vim8]($resource/vim8.jpg)


end

