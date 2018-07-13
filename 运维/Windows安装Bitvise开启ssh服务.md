Windows7安装Bitvise开启ssh服务
===
[[toc]]

by:铁乐猫

在Liunx和windows10上配置SSH服务是一件很容易的事，毕竟系统己经自带了ssh的服务功能。
不过在windows7上可不容易，也有几种实现的方案，今天要说的就是通过安装Bitvise这个软件来在windows7上搭建出ssh服务。
其实在windows7上搭建ssh服务，对于我来讲，是因为刚巧在学习mysql，而又是在自己使用的笔记本上win7系统上安装了mysql。虽然使用cmd命令窗口来连接和运行mysql的命令也无所谓。不过我当时想的是想用xshell连接上本地ssh服务后再连接上mysql来操作，这样可以方便用xshell的历史日记来查看操作记录。

那么，接下来就开始安装Bitvise吧。
首先需要翻墙才能访问到Bitvise的官网网站，进而在官网上下载。
官网链接：https://www.bitvise.com/

![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_download.jpg)

官网上有提供客户端和服务端，两者我都下载回来安装使用过。
客户端也挺好用的，就是缺少了像xshell那样的历史日记。
但它同时还集成了sftp，windows远程桌面等客户端服务。

## 安装Bitvise SSH Server

* 点击下载回来的安装包
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_install_01.jpg)

点选同意协议，不想改变默认安装路径的话就可以直接点击**Install**了。

* 出现版本选择，因为是个人用户，且功能也己足够使用了，所以点击下方的**Personal Edition**。
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_install_02.jpg)

个人版有功能限制，但胜在永久免费。标准版则是30天试用。

* 填写上个人信息，点击**Ok**
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_install_03.jpg)

* 随后程序开始正式安装，弹出的是安装进行的命令行提示界面。
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_install_04.jpg)

* 安装完成弹出的提示框。
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_install_05.jpg)

## 配置Bitvise SSH Server

* Bitvise ssh 服务端的控制面板界面如下

![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_ssh01.jpg)

点击**Settings**栏的**Open easy settings**进行简易配置。

* 简易配置界面有三个主要选项卡，分别是1 服务设置，2 windows系统用户设置，3 虚拟用户设置；
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_ssh02.jpg)

可以在1 服务设置中设置IP协议和使用的ssh端口，默认启用22端口。

* 我主要使用的虚拟用户设置，因为这样可以自由定义，不用像第二项中一样要和windows系统用户关联起来。

![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_ssh03.jpg)

如上图，设置好用户名，密码，虚拟根目录。

* 设置好用于连接ssh服务的虚拟用户后，返回ssh服务控制面板主界面，点击**Start Server**就可对windows7本机开启ssh服务了。
![](https://www.tielemao.com/wp-content/uploads/2018/06/Btivise_ssh04.jpg)

## 使用xshell连接ssh服务验证

* 使用xshell直接连接127.0.0.1:22，连接成功
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_ssh05.jpg)

* 连接后可看到环境变量是Bitivse SSH Server虚拟出来的，所以原本在windwos7系统上安装好的很多软件服务等不能直接启动成功。
![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_ssh06.jpg)

例如我要连接上mysql就需要进到mysql的安装目录下启动mysql客户端。

* 同样，在**Bitvise SSH Server Control Panel**中，可以通过**Activity** 选项卡中看到客户端的连接日志。

![](https://www.tielemao.com/wp-content/uploads/2018/06/Bitvise_ssh07.jpg)

## 结语

* 附Bitvise SSH Client 连接ssh服务端用的客户端
![](https://www.tielemao.com/wp-content/uploads/2018/06/BitviseSshClient.jpg)

可以说Bitvise设计得很易懂，即使不汉化使用也完全没有问题。
当然毕竟因为是虚拟出来的连接环境，就环境变量而言有点缺陷而己。
能做到实现了ssh连接也己经是难能可贵了。

今次对它的使用和介绍就简短到此吧。相信它还有很多功能值得去发掘。

【end】
2018-6-19

