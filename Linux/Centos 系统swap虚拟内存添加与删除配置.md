# Centos 系统swap虚拟内存添加与删除配置

https://blog.csdn.net/forlong401/article/details/42129175

1.swap概述

    Swap分区，即交换区，Swap空间的作用可简单描述为：当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap空间中，等到那些程序要运行时，再从Swap中恢复保存的数据到内存中。这样，系统总是在物理内存不够时，才进行Swap交换。 其实，Swap的调整对Linux服务器，特别是Web服务器的性能至关重要。通过调整Swap，有时可以越过系统性能瓶颈，节省系统升级费用。

2.创建swap

[forlong401@dfdsfdsfs ~]# cd /var/

获取要增加的SWAP文件块

[forlong401@dfdsfdsfs var]# dd if=/dev/zero of=swapfile bs=1024 count=2048000

记录了2048000+0 的读入

记录了2048000+0 的写出

2097152000字节(2.1 GB)已复制，61.357 秒，34.2 MB/秒

创建SWAP文件

[forlong401@dfdsfdsfs var]# mkswap swapfile

mkswap: swapfile: warning: don't erase bootbits sectors

        on whole disk. Use -f to force.

Setting up swapspace version 1, size = 2047996 KiB

no label, UUID=fdafdsfdsb-3sfdb-4sfdc-bfdfd7-8sdfsafds89

激活SWAP文件

[forlong401@dfdsfdsfs var]# swapon swapfile

查看SWAP信息是否正确

[forlong401@dfdsfdsfs var]# swapon -s

Filename  Type  Size  Used  Priority

/var/swapfile                           file  2047996  0  -1

添加到fstab文件中让系统引导时自动启动

[forlong401@dfdsfdsfs var]# echo "/var/swapfile swap swap defaults 0 0">>/etc/fstab

[forlong401@dfdsfdsfs var]# free -m

             total       used       free     shared    buffers     cached

Mem:           488        478         10          0          4        132

-/+ buffers/cache:        341        147

Swap:         1999          0       1999

[forlong401@dfdsfdsfs var]# top -a

top - 21:04:55 up 3 days, 21:06,  2 users,  load average: 0.05, 0.25, 0.13

Tasks: 113 total,   1 running, 112 sleeping,   0 stopped,   0 zombie

Cpu(s):  0.3%us,  0.0%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st

Mem:    52423423k total,   424234232k used,     942343k free,     523424k buffers

Swap:  2047996k total,        0k used,  2047996k free,   136088k cached

3.删除swap分区

       有时可能会需要删除swap分区，该如何正确进行删除分区哪?

*   首先停止swap分区

    1.  swapoff   /swap/swap

*   删除swap分区文件

    1.  rm -rf /swap/swap

*   删除"/etc/swap"指定文件

    1.  sed  -i "/'\/swa\/swap   swap   swap  defaults 0 0'//" /etc/fstab

     这样就可以手工添加和删除swap分区。