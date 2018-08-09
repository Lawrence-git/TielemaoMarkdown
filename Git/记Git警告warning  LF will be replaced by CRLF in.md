# 记Git警告warning  LF will be replaced by CRLF in

   Windows上，对一个项目（包含很多代码文件）使用Git时，输入添加命令（例如：git add .）时，很有可能会出现`warning: LF will be replaced by CRLF in……`类似的警告。

**原因分析：**

   `CRLF -- Carriage-Return Line-Feed `表示回车换行。
   也就是回车（`CR, ASCII 13, \r`），换行（`LF, ASCII 10,\n`）。这两个ACSII字符不会在屏幕有任何显示，是Windows平台上用来标识一行的结束。
   而在Linux/UNIX系统中只有换行符LF，没有回车符CR。也就是说在Windows中的换行符为 CRLF，而在Linux下的换行符为：LF。
   工作区的文件都应该用 CRLF 来换行。如果改动文件时引入了 LF,提交改动时，git 会警告你哪些文件不是纯 CRLF 文件，但 git 不会擅自修改工作区的那些文件，而是对暂存区（我们对工作区的改动）进行修改。也因此，当我们进行 git add 的操作时，只要 git 发现改动的内容里有 LF 换行符，就还会出现这个警告。
   因此使用Git来生成一个工程后，文件中的换行符为LF，当执行添加命令（例如：git add .）时，系统就会发出警告。当最终push到远程仓库的时候git会统一格式全部转化为用CRLF作为换行符

**解决方法：**
   根据实际需要进行配置选项。
   一般会把`core.autocrlf` 设置成`false`
```git
1.  git config –global core.autocrlf true 
# 进行回车符转换，默认
2.  git config –global core.autocrlf input 
# 上库转换，从库中迁出代码时不转换
3.  git config –global core.autocrlf false 
# 一般是window平台上的，可以设置为不转换
```

**建议：**
这种规范问题也可以直接忽略，对整体工作不会造成影响！
