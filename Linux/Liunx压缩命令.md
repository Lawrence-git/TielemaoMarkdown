# linux压缩命令

减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。gzip是在Linux系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。gzip不仅可以用来压缩大的、较少使用的文件以节省磁盘空间，还可以和tar命令一起构成Linux操作系统中比较流行的压缩文件格式。据统计，gzip命令对文本文件有60%～70%的压缩率。

1．命令格式：

gzip[参数][文件或者目录]

2．命令功能：

gzip是个使用广泛的压缩程序，文件经它压缩过后，其名称后面会多出".gz"的扩展名。

3．命令参数：

-a或--ascii 　使用ASCII文字模式。 

-c或--stdout或--to-stdout 　把压缩后的文件输出到标准输出设备，不去更动原始文件。 

-d或--decompress或----uncompress 　解开压缩文件。 

-f或--force 　强行压缩文件。不理会文件名称或硬连接是否存在以及该文件是否为符号连接。 

-h或--help 　在线帮助。 

-l或--list 　列出压缩文件的相关信息。 

-L或--license 　显示版本与版权信息。 

-n或--no-name 　压缩文件时，不保存原来的文件名称及时间戳记。 

-N或--name 　压缩文件时，保存原来的文件名称及时间戳记。 

-q或--quiet 　不显示警告信息。 

-r或--recursive 　递归处理，将指定目录下的所有文件及子目录一并处理。 

-S<压缩字尾字符串>或----suffix<压缩字尾字符串> 　更改压缩字尾字符串。 

-t或--test 　测试压缩文件是否正确无误。 

-v或--verbose 　显示指令执行过程。 

-V或--version 　显示版本信息。 

-num 用指定的数字num调整压缩的速度，-1或--fast表示最快压缩方法（低压缩比），-9或--best表示最慢压缩方法（高压缩比）。系统缺省值为6。 
That's it .<love-love-love@live.cn>  10:28:48
linux下最常用的打包程序就是tar了，使用tar程序打出来的包我们常称为tar包，tar包文件的命令通常都是以.tar结尾的。生成tar包后，就可以用其它的程序来进行压缩。

1．命令格式：

tar[必要参数][选择参数][文件] 

2．命令功能：

用来压缩和解压文件。tar本身不具有压缩功能。他是调用压缩功能实现的 

3．命令参数：

必要参数有如下：

-A 新增压缩文件到已存在的压缩

-B 设置区块大小

-c 建立新的压缩文件

-d 记录文件的差别

-r 添加文件到已经压缩的文件

-u 添加改变了和现有的文件到已经存在的压缩文件

-x 从压缩的文件中提取文件

-t 显示压缩文件的内容

-z 支持gzip解压文件

-j 支持bzip2解压文件

-Z 支持compress解压文件

-v 显示操作过程

-l 文件系统边界设置

-k 保留原有文件不覆盖

-m 保留文件不被覆盖

-W 确认压缩文件的正确性

可选参数如下：

-b 设置区块数目

-C 切换到指定目录

-f 指定压缩文件

--help 显示帮助信息

--version 显示版本信息

4．常见解压/压缩命令

tar 
解包：tar xvf FileName.tar
打包：tar cvf FileName.tar DirName
（注：tar是打包，不是压缩！）

.gz
解压1：gunzip FileName.gz
解压2：gzip -d FileName.gz
压缩：gzip FileName

.tar.gz 和 .tgz
解压：tar zxvf FileName.tar.gz
压缩：tar zcvf FileName.tar.gz DirName

.bz2
解压1：bzip2 -d FileName.bz2
解压2：bunzip2 FileName.bz2
压缩： bzip2 -z FileName

.tar.bz2
解压：tar jxvf FileName.tar.bz2
压缩：tar jcvf FileName.tar.bz2 DirName

.bz
解压1：bzip2 -d ![](file:///C:\Users\ADMINI~1\AppData\Local\Temp\%W@GJ$ACOF(TYDYECOKVDYB.png)FileName.bz
解压2：bunzip2 ![](file:///C:\Users\ADMINI~1\AppData\Local\Temp\%W@GJ$ACOF(TYDYECOKVDYB.png)FileName.bz
压缩：未知

.![](file:///C:\Users\ADMINI~1\AppData\Local\Temp\%W@GJ$ACOF(TYDYECOKVDYB.png)tar.bz
解压：tar jxvf ![](file:///C:\Users\ADMINI~1\AppData\Local\Temp\%W@GJ$ACOF(TYDYECOKVDYB.png)FileName.tar.bz
压缩：未知

.Z
解压：uncompress FileName.Z
压缩：compress FileName

.tar.Z
解压：tar Zxvf FileName.tar.Z
压缩：tar Zcvf FileName.tar.Z DirName

.zip
解压：unzip FileName.zip
压缩：zip FileName.zip DirName

.rar
解压：rar x FileName.rar
压缩：rar a FileName.rar DirName