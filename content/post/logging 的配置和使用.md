+++ 
title = "Python 中logging 模块的配置和使用" 
date = "Sun, 25 Dec 2016 04:49:00 GMT" 
categories = ["配置"] 
description = "Python 中logging 模块的使用记录" 
+++ 

reference :

- [logging cookbook](http://python.usyiyi.cn/python_278/library/logging.handlers.html#)
- [logging HOWTO](http://python.usyiyi.cn/translate/python_278/howto/logging.html#logging-advanced-tutorial)

测试源码，example

```
import logging

nt = 'xwei'
# logging.basicConfig(filename='log/log_test.log',
#                     format='%(asctime)s, %(levelname)s:%(message)s', datefmt='%m/%d/%Y %I:%M:%S %p',
#                     filemode='w', level=logging.INFO)

# set up logging to file - see previous section for more details
# w ,new file
logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s %(name)-12s %(levelname)-8s %(message)s',
                    datefmt='%m/%d/%Y %H:%M:%S %p',
                    filename='log/log_test.log',
                    filemode='w')
# define a Handler which writes INFO messages or higher to the sys.stderr
console = logging.StreamHandler()
console.setLevel(logging.INFO)
# set a format which is simpler for console use
formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
# tell the handler to use this format
console.setFormatter(formatter)
# add the handler to the root logger
logging.getLogger('').addHandler(console)

# Now, we can log to the root logger, or any other logger. First the root...
logging.info('Jackdaws love my big sphinx of quartz.')

# Now, define a couple of other loggers which might represent areas in your
# application:
logger1 = logging.getLogger('myapp.area1')
logger2 = logging.getLogger('myapp.area2')

logger1.debug('Quick zephyrs blow, vexing daft Jim.')
logger1.info('How quickly daft jumping zebras vex.')
logger2.warning('Jail zesty vixen who grabbed pay from quack.')
logger2.error('The five boxing wizards jump quickly.')

# 'application' code
logging.debug("this is debug.")
logging.info("this is info form %s." % nt)
logging.warning("this is warning from %s." % nt)

```

默认设置文件的level 是debug，基础上再设置console的格式。

程序分别在文件中和console里面保留了对应level的信息：

- console 记录显示大于info level的信息
- 文件中记录level 超过 debug 的信息


####  level设计
logging函数根据它们用来跟踪的事件的级别或严重程度来命名。[标准级别及其适用性描述](http://python.usyiyi.cn/translate/python_278/howto/logging.html#logging-advanced-tutorial)如下（以严重程度递增排序）：

级别       | 数字值 | 何时使用
------|---|--------
DEBUG    | 10 |详细信息，典型地调试问题时会感兴趣。
INFO     | 20 |证明事情按预期工作。
WARNING  | 30 |表明发生了一些意外，或者不久的将来会发生问题（如‘磁盘满了’）。软件还是在正常工作。
ERROR    | 40 |由于更严重的问题，软件已不能执行一些功能了。
CRITICAL | 50 |严重错误，表明软件已不能继续运行了。


默认级别为 WARNING，表示只有该级别及其以上的事件会被跟踪，除非另外配置了logging包。

我希望：

- 在console里面输出所有的交互信息，用debug level；
- 在文件里面保存每次迭代完成的数据，用于分析画图，不要太多的中间数据，应该用info level。
- logging 可以配置[RotatingFileHandler保留循环文件的大小以及个数](http://python.usyiyi.cn/python_278/library/logging.handlers.html#)，不想保存太多的log信息。



