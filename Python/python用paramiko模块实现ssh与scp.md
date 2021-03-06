# python用paramiko模块实现ssh与scp

# 1\. 介绍

这篇文章简单地介绍了python的paramiko模块的用法，paramiko实现了SSH协议，能够方便地与远程计算机交互。简单的说，就是你在terminal下执行的如下语句，现在可以通过python的paramiko实现了。

```
    # 执行shell语句
    ssh -i ~/.ssh/id_rsa -p 1098  rds@12.164.145.21 -e 'ls -al'

    # 拷贝数据到远程计算机
    scp -i ~/.ssh/id_rsa -P 1098 -r data rds@12.164.145.21:~/data
```

这里不讨论shell与python实现的优缺点，如果你没有需求，也不会看到这篇博客了。我个人使用paramiko是为了使用python的多线程，并发地对多台远程计算机执行相同的操作。

这篇博客虽然篇幅不大，但是，可能是目前网络上最好的中文入门教程了。那就开始吧！

# 2\. 安装

安装非常简单，直接使用pip安装即可:

```
sudo pip instal paramiko
```

# 3\. 建立SSH连接

使用密码连接：

```
import paramiko
ssh = paramiko.SSHClient()

#这行代码的作用是允许连接不在know_hosts文件中的主机。
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect("IP",  port, "username", "password")
```

使用私钥连接：

```
ssh = paramiko.SSHClient()
ssh.connect('10.120.48.109', port, '用户名',
key_filename='私钥')
```

连接以后可以执行shell命令：

```
In [8]: ssh.exec_command('ls')
Out[8]:
(<paramiko.ChannelFile from <paramiko.Channel 1 (open) window=2097152 -> <paramiko.Transport at 0x377c690L (cipher aes128-ctr, 128 bits) (active; 2 open channel(s))>>>,
 <paramiko.ChannelFile from <paramiko.Channel 1 (open) window=2097152 -> <paramiko.Transport at 0x377c690L (cipher aes128-ctr, 128 bits) (active; 2 open channel(s))>>>,
 <paramiko.ChannelFile from <paramiko.Channel 1 (open) window=2097152 -> <paramiko.Transport at 0x377c690L (cipher aes128-ctr, 128 bits) (active; 2 open channel(s))>>>)
```

执行shell命令以后，并不会立即打印命令的执行结果，而是返回几个Channel, 只能像下面这样获取输出：

```
In [9]: stdin, stdout, stderr = ssh.exec_command('ls')

In [10]: print stdout.readlines()
['AgentBackkup_2015-06-11\n', 'AgentBackup\n', 'log\n', 'mysql.sh\n', 'rdsAgent\n']
```

**注意：** 命令执行出错并不会抛出异常，所以，对于命令出错需要根据自己的需求进行相应的处理：

```
In [54]: stdin, stdout, stderr = ssh.exec_command('cat file_not_found')
In [55]: print stdout.readlines()
[]

In [56]: print stderr.readlines()
[u'cat: file_not_found: No such file or directory\n']

In [57]: stdin, stdout, stderr = ssh.exec_command('ls')
In [58]: print stderr.readlines()
[]
```

**API文档:** [https://paramiko-docs.readthedocs.org/en/1.15/api/client.html](https://paramiko-docs.readthedocs.org/en/1.15/api/client.html)

# 4\. SCP vs SFTP

通过paramiko还可以传输文件，这是我写这篇博客的主要原因。搜了很多博客，都没有说明白如何通过paramiko在计算机之间传输文件，通过阅读官方文档，发现有如下两种方式：

```
sftp = paramiko.SFTPClient.from_transport(ssh.get_transport())
sftp = ssh.open_sftp()
```

即新建一个SFTPClient对象，该对象复用之前的SSH连接，因此，我们使用sftp传输文件时，不需要再次进行用户认证。

*   文件上传

    ```
      In [59]: sftp.put('memory.py', 'memory.py')
      Out[59]: <SFTPAttributes: [ size=288 uid=1000 gid=1000 mode=0100644 atime=1435391914 mtime=1435391914 ]>
    ```

*   文件下载

    ```
      In [60]: sftp.get('memory.py', 'backup.py')
    ```

*   执行命令

    paramiko并没有提供一个叫做scp的子模块，如果我们希望在计算机之间传输数据，可以通过sftp(sftp实现了scp所有的功能，也就没有必再实现一个scp)传输文件，还可以通过sftp执行命令，如下所示：

    ```
      In [44]: sftp.listdir()
      Out[44]:
      ['.viminfo',
      '.bash_logout',
      '.bash_history',
      'AgentBackkup_2015-06-10',
      'AgentBackup',
      'rdsAgent']

      In [45]: sftp.rename('AgentBackkup_2015-06-10', 'AgentBackkup_2015-06-11')

      In [46]: sftp.listdir()
      Out[46]:
      ['AgentBackkup_2015-06-11',
      '.viminfo',
      '.bash_logout',
      '.bash_history',
      'AgentBackup',
      'rdsAgent']
    ```

sftp提供了很多命令，具体内容可以参考[官方文档](https://paramiko-docs.readthedocs.org/en/1.15/api/sftp.html) 。