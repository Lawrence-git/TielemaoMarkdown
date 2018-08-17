# Python日志功能详解
https://blog.igevin.info/posts/python-log/

![logging-python-watermarkgrey]($resource/logging-python-watermarkgrey.png)

软件开发中通过日志记录程序的运行情况是一个开发的好习惯，对于错误排查和系统运维都有很大帮助。Python标准库自带日志模块，已经足够强大，大部分情况下，python程序的日志功能直接调用标准库的日志模块即可。《The Hitchhiker’s Guide to Python》已对“日志”进行了[详细阐述](http://docs.python-guide.org/en/latest/writing/logging/)，python的官方文档也对日志做了[说明](https://docs.python.org/2/howto/logging.html)，但Gevin依然感觉，通过这些英文资料，还不能让初学者在短时间迅速掌握python日志模块的使用，因此按照自己的思路，把相关内容做了如下整理，并附加一些Gevin认为文档中没有说明清楚的内容。

# 1\. 基本用法

如果开发轻量级的应用，对日志的需求也比较简单，直接参考如下示例，在相关代码逻辑中加入日志功能即可：

```
#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import logging

logging.debug('debug message')
logging.info('info message')
logging.warn('warn message')
logging.error('error message')
logging.critical('critical message')

```

output:

```
WARNING:root:warn message
ERROR:root:error message
CRITICAL:root:critical message

```

默认情况下，logging模块将日志打印到屏幕上，日志级别为WARNING(即只有日志级别等于或高于WARNING的日志信息才会输出)，日志格式为 `warning level:instance name:warning message`

## 1.1 将日志记录到文件中

将日志记录到文件中，只需在调用logging模块记录日志前，做个简单的配置即可：

```
#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import logging

# 配置日志文件和日志级别
logging.basicConfig(filename='logger.log', level=logging.INFO)

logging.debug('debug message')
logging.info('info message')
logging.warn('warn message')
logging.error('error message')
logging.critical('critical message')

```

本样例在记录日志前，通过`logging.basicConfig`做简单配置，将日志记录到日志文件`logger.log`中，也修改了默认的日志等级，高于或等于INFO级别的日志信息，都会记录到日志文件中。

# 2\. 更加完善的日志功能

## 2.1 几个关键概念

如果要更加灵活的使用日志模块，首先要了解日志模块是怎样工作的。 `Logger`，`Handler`，`Formatter`和`Filter`是日志模块的几个基本概念，日志模块的工作原理要从这四个基本概念说起。

*   Logger 即记录器，Logger提供了日志相关功能的调用接口。
*   Handler 即处理器，将（记录器产生的）日志记录发送至合适的目的地。
*   Filter 即过滤器，提供了更好的粒度控制，它可以决定输出哪些日志记录。
*   Formatter 即格式化器，指明了最终输出中日志记录的格式。

### 2.1.1 Logger

Logger 即“记录器”，Logger对象实例是日志记录功能的载体，如：

```
#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import logging

logger = logging.getLogger('simple_example')

logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')

```

值得一提的是，Logger对象从不直接实例化，而是通过模块级的功能`logging.getLogger(name)`创建Logger实例。调用 `logging.getLogger(name)` 功能时，如果传入的`name`参数值相同，则总是返回同一个Logger对象实例的引用。

如果没有显式的进行创建，则默认创建一个root logger，并应用默认的日志级别(WARN)、默认的处理器Handler(StreamHandler，即将日志信息打印输出在标准输出上)，和默认的格式化器Formatter(默认的格式即为第一个简单使用程序中输出的格式)。

Logger类包含的成员和方法可以查看[官方文档](https://docs.python.org/2/library/logging.html#logger-objects)。

### 2.1.2 Handler

Handler 将日志信息发送到设置的位置，可以通过Logger对象的`addHandler()`方法为Logger对象添加0个或多个handler。一种日志的典型应用场景为，系统希望将所有的日志信息保存到log文件中，其中日志等级等于或高于`ERROR`的消息还要在屏幕标准输出上显示，日志等级为`CRITICAL`的还需要发送邮件通知；这种场景就需要3个独立的handler来实现需求，这三个handler分别与指定的日志等级或日志位置做响应

需要一提的是，为Logger配置的handler不能是Handler基类对象，而是Handler的子类对象，常用的Handler为`StreamHandler`, `FileHandler`， 和`NullHandler`，Handler的全部子类及详细介绍可以查看[官方文档相应页面](https://docs.python.org/2/library/logging.handlers.html#module-logging.handlers)如果需要了解更多关于Handler的信息，直接查看[官方文档](https://docs.python.org/2/library/logging.html#handler-objects)即可。

### 2.1.3 Formatter

Formatter 用于设置日志输出的格式，与前两个基本概念不同的是，该类可以直接初始化对象，即 `formatter=logging.Formatter(fmt=None, datefmt=None)`，创建formatter时，传入分别`fmt`和`datefmt`参数来修改日志格式和时间格式，默认的日志格式为`%(asctime)s - %(levelname)s - %(message)s`，默认的时间格式为`%Y-%m-%d %H:%M:%S`

### 2.1.4 Filter

Filter 可用于Logger对象或Handler对象，用于提供比日志等级更加复杂的日志过滤方式。默认的filter只允许在指定logger层级下的日志消息通过过滤。例如，如果把filter设置为`filter=logging.Filter('A.B')`，则logger ‘A.B’, ‘A.B.C’, ‘A.B.C.D’, ‘A.B.D’ 产生的日志信息可以通过过滤，但'[A.BB](http://A.BB)', 'B.A.B'均不行。如果以空字符串初始化filter，则所有的日志消息都可以通过过滤。

Filter在日志功能配置中是非必须的，只在对日志消息过滤需求比较复杂时配置使用即可。

## 2.2 日志产生流程

日志产生的流程逻辑参考下图即可：

![logging flow](http://gevin-zone.igevin.info/gevinblog/image/logging_flow.png?imageView2/2/w/800/interlace/0/q/100 "logging flow")

## 2.3 日志模块的使用

日志模块使用的关键是“日志的配置”，日志配置好后，只要调用`[logger.INFO](http://logger.INFO)()`, `logger.ERROR()`等方法即可创建日志内容。

开发者可以通过三种方法配置日志模块：

1.  在Python代码中显示创建loggers, handlers, formatters甚至filters，并调用这几个对象中的各个配置函数来完成日志配置
2.  将配置信息写到配置文件中，然后读取配置文件信息来完成日志配置
3.  将配置信息写到一个`Dict`中，然后读取这个配置字典来完成日志配置

### 2.3.1 通过代码配置并使用日志模块

通过代码配置日志模块简单方便，但如果需要修改配置时，需要改代码，因此不建议在大型项目中使用这种方法。

通过代码配置日志模块可以很好的理解日志模块的工作原理，用于学习，是一个很好的案例，因此Gevin也在下文中对此详细介绍。

1\. 创建Logger

```
import logging

# create logger
logger = logging.getLogger('simple_example')

# Set default log level
logger.setLevel(logging.DEBUG)

```

2\. 创建Handler

```

# create console handler and set level to warn

ch = logging.StreamHandler()
ch.setLevel(logging.WARN)

```

3\. 创建Fomatter

```
# create formatter
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

```

4\. 配置Logger

```
# add formatter to ch
ch.setFormatter(formatter)

# add ch to logger
# The final log level is the higher one between the default and the one in handler
logger.addHandler(ch)

```

5\. 使用日志模块

```
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')

```

6\. **完整的例子**

```

#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import logging

# create logger
logger = logging.getLogger('simple_example')

# Set default log level
logger.setLevel(logging.DEBUG)

ch = logging.StreamHandler()
ch.setLevel(logging.WARN)

ch2 = logging.FileHandler('logging.log')
ch2.setLevel(logging.INFO)

# create formatter
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# add formatter to ch
ch.setFormatter(formatter)
ch2.setFormatter(formatter)

# add ch to logger
# The final log level is the higher one between the default and the one in handler
logger.addHandler(ch)
logger.addHandler(ch2)

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')

```

### 2.3.2 通过配置文件配置并使用日志模块

通过配置文件配置日志模块时，配置文件通常使用`.ini`格式，日志模块需要调用`fileConfig`，即`logging.config.fileConfig('logging_config.ini')`，然后logger的使用方法与上面相同：

```

#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import logging
import logging.config

logging.config.fileConfig('logging_config.ini')

# create logger
logger = logging.getLogger('root')

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')

```

其中，`logging_config.ini`文件内容如下：

```
[loggers]
keys=root,simpleExample

[handlers]
keys=consoleHandler

[formatters]
keys=simpleFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_simpleExample]
level=INFO
handlers=consoleHandler
qualname=simpleExample
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)

[formatter_simpleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s

```

通过配置文件配置日志模块，逻辑与代码中配置一样，也是把`logger`, `handler`和`formatter`定义好，然后组装到一起即可，无非ini配置和代码配置时的语法不通而已，开发者在基于ini文件配置日志模块时，只要参考上面例子做相应修改即可。

### 2.3.3 通过Dict对象配置并使用日志模块

基于Dict对象配置日志模块在python中应用广泛，很多Django或Flask项目都采用这种方式，但很多官方文档对这种方法介绍并不多，因此，本文提供一个使用样例，以后开发中参考该样例修改一下即可。

```

#!/usr/local/bin/python
# -*- coding: utf-8 -*-

import logging
import logging.config

config = {
    'version': 1,
    'formatters': {
        'simple': {
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        },
    },
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
            'level': 'DEBUG',
            'formatter': 'simple'
        },
        'file': {
            'class': 'logging.FileHandler',
            'filename': 'logging.log',
            'level': 'DEBUG',
            'formatter': 'simple'
        },
    },
    'loggers':{
        'root': {
            'handlers': ['console'],
            'level': 'DEBUG',
            # 'propagate': True,
        },
        'simple': {
            'handlers': ['console', 'file'],
            'level': 'WARN',
        }
    }
}

logging.config.dictConfig(config)

print 'logger:'
logger = logging.getLogger('root')

logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')

print 'logger2:'
logger2 = logging.getLogger('simple')

logger2.debug('debug message')
logger2.info('info message')
logger2.warn('warn message')
logger2.error('error message')
logger2.critical('critical message')

```

# 注:

## 日志的严重等级

Log Level如下，严重等级为`NOTSET`, `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`, 严重程度依次递增

```
CRITICAL: 50
ERROR: 40
WARNING: 30
INFO: 20
DEBUG: 10
NOTSET: 0

```

## 修改日志消息的格式

日志的默认显示格式为：`%(asctime)s - %(name)s - %(levelname)s - %(message)s`，如果只想显示日志等级和日志信息，可以把格式改为：`%(levelname)s:%(message)s`，想了解全部Formatter中的可用变量，请查阅[LogRecord attributes](https://docs.python.org/2/library/logging.html#logrecord-attributes)

日期时间的默认格式是ISO8601，修改日期时间格式请参考 [time.strftime()](https://docs.python.org/2/library/time.html#time.strftime)