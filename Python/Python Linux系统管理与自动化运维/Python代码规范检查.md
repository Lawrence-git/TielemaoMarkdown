# Python代码规范检查

## PEP8编码规范

例：包导入
* 在python中，import应该一次只导入一个模块。
* 不同的模块应该独立一行。
* 如果要从一个模块导入多个API，可以像下面：
`from subprocess import Popen, PIPE`
* import语句应该处于源码文件的顶部，位于模块注释和文档字符串之后，全局变量和常量之前。
* 导入不同的库时，应该按以下顺序分组，各个分组之间以空行分隔：
  * 1）导入标准库模块；
  * 2）导入相关第三方库模块；
  * 3）导入当前应用程序/库模块。
* python中支持相对导入和绝对导入，推荐使用绝对导入。
  * 绝对导入可读性更好，不容易出错，即使出错也会给出更详细的错误信息。
* 如果是处理复杂的包结构，绝对导入可能会显得比较冗余，这个时候可以使用相对导入。
* 避免使用通配符导入。通配符导入会使名称空间里存在的名称变得不清晰，迷惑读者和自动化工具。

### 使用pycodestyle检查代码规范
* 安装
`pip install pycodestyle`
* 打印检查报告
例：
`pycodestyle --first optparse.py`
* 显示不符合规范的源码，以便进行修正
例：
`pycodestyle --show-source --show-pep8 testsuite/E40.py`

### 使用autopep8将代码格式化
autopep8能够将python代码自动格式化为PEP8风格。
autopep8使用pycodestyle工具来决定代码中的哪部分需要被格式化，这能够修复大部分pycodestyle工具中报告的排版问题。

* 安装
`pip install autopep8`

* 使用
`autopep8 --in-place optparse.py`
`--in-place`类似于sed命令的`-i`选项，如果不包含`--in-place`选项，则会将autopep8格式化以后的代码直接输出到控制台。