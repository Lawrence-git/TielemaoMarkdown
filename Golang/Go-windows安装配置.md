Go-windows安装配置
===

## 前言

学习完了python基础，顺便也要提前学习一下go啦，抱着这样的心情，今晚尝试了安装一下go，很顺利的完成了，没有难度。

需要了解更多的关于Go的基本信息可以去[维基百科](https://zh.wikipedia.org/wiki/Go)查看信息，这里就不做描述了。

Go官方网站：[https://golang.org](https://golang.org)
Go官网文档：[https://golang.org/doc](https://golang.org/doc)

## 安装-windows篇

安装以便使用Go很简单，只需要安装它的编译器就可以了：

到官网[下载地址](https://golang.org/dl/)下载windows所用的系统安装包，下载回来后点击安装即可。

![](https://www.tielemao.com/wp-content/uploads/2018/05/go-msi.jpg)

选择下载msi包来进行安装，安装进行的同时还会替你设置好环境变量等。

如果是`zip`解压的需要配置下环境变量，此过程不再描述。如果是`msi`安装包会自动配置环境变量，检验是否能正常使用只需要打开`cmd`或者`powershell`输入： go version

```cmd
C:\Users\Administrator>go version
go version go1.10.2 windows/amd64
```
返回go的版本信息则正常！

## GOPATH设置

仅仅安装好msi是不够的，还需要配置一些东西：`GOPATH`

Go从1.1版本到1.7必须设置这个变量，而且不能和Go的安装目录一样。

这个目录用来存放Go源码，Go的可运行文件，以及相应的编译之后的包文件。

所以这个目录下面有三个子目录：src、bin、pkg

从go 1.8开始，GOPATH环境变量现在有一个默认值，如果它没有被设置。 它在Unix上默认为`$HOME/go`,在Windows上默认为`%USERPROFILE%/go`。

如果你的是Unix环境需要：

```bash
export GOPATH=/home/tielemao/GoWork

```
Windows则需要新建一个环境变量名称叫做GOPATH：

```bash
GOPATH=E:\GoWork
```
当然，go 的msi安装包安装完成后，己默认设置了GOPATH，我们只需要右击【计算机】属性-【高级系统设置】-【高级】-【环境变量】设置编辑就可以了。

![](https://www.tielemao.com/wp-content/uploads/2018/05/Go-work.jpg)

GOPATH允许多个目录，当有多个目录时，请注意分隔符，多个目录的时候Windows是分号，Linux系统是冒号，当有多个GOPATH时，默认会将go get的内容放在第一个目录下。

以上 $GOPATH 目录约定有三个子目录：

*   src 存放源代码（比如：.go .c .h .s等）
*   pkg 编译后生成的文件（比如：.a）
*   bin 编译后生成的可执行文件（为了方便，可以把此目录加入到 `$PATH` 变量中，如果有多个gopath，那么使用`${GOPATH//://bin:}/bin`添加所有的bin目录）

### hello world

配置好后，进一步构建一个简单的实例去检查go是否正常。
在你的上文的`GOPATH`新建一个`src/hello/hello.go`文件，内容如下：

```go
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}
```
然后使用终端进入该文件夹，并且构建编译：
例：我的GOPATH设置的是E盘下的GoWork目录。

```cmd
E:\> cd GoWork\src\hello
E:\GoWork\src\hello> go build
```
注： go 前面还有个空格才能正常执行build命令。

编译完成后，可以dir命令看到该目录下有个`hello.exe`可执行文件。
继续在终端执行：

```cmd
E:\GoWork\src\hello> hello
hello, world

```
则会输出`hello world`。

![go-hello]($res/go-hello.jpg)

至此，GO在windows系统上安装完成。

### GoDocServer

![](https://www.tielemao.com/wp-content/uploads/2018/05/GoDocServer.jpg)

附：点击开始菜单，所有程序，找到go的程序目录，它下面会有一个GoDocServer程序，运行后的效果如上图，也是挺有意思的。可以在web界面下查看整个Go目录树了。

end
2018-05-30 星期三
