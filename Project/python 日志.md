
### scrapy中使用日志

```
1. setting.py文件中添加

LOG_LEVEL = 'ERROR' # 日志显示等级
LOG_FILE = './log.log' # 日志保存的目录

#CRITICAL - 严重错误
#ERROR - 一般错误
#WARNING - 警告信息
#INFO - 一般信息
#DEBUG - 调试信息

2.显示log位置，在pipelines.py加入

import logging

logger = logging.getLogger(__name__)

def process_item(self, item, spider):
    logger.warning(item)
 ```   
### python loguru 日志模块简单使用
参考链接：https://blog.csdn.net/mouday/article/details/88560543
```
#from loguru import logger
#logger.add("file_{time}.log")

#logger.debug("这是一条debug日志")
#logger.info("这是一条info日志")

from loguru import logger

logger.add("file.log", format="{time:YYYY-MM-DD at HH:mm:ss} | {level} | {message}", filter="", level="DEBUG")

logger.debug("这是一条debug日志")
logger.info("这是一条info日志")

#logger.add("file_1.log", rotation="500 MB")    # 文件过大就会重新生成一个文件
#logger.add("file_2.log", rotation="12:00")     # 每天12点创建新文件
#logger.add("file_3.log", rotation="1 week")    # 文件时间过长就会创建新文件

#logger.add("file_X.log", retention="10 days")  # 一段时间后会清空

#logger.add("file_Y.log", compression="zip")    # 保存zip格式
#logger.add("file.log", format="{time:YYYY-MM-DD at HH:mm:ss} | {level} | {message}")    # 时间格式化
```

### 保存控制台输出的内容
```
import sys
class Logger(object):
    def __init__(self, filename='default.log', stream=sys.stdout):
	    self.terminal = stream
	    self.log = open(filename, 'a')

    def write(self, message):
	    self.terminal.write(message)
	    self.log.write(message)

    def flush(self):
	    pass

sys.stdout = Logger('a.log', sys.stdout)
# sys.stderr = Logger('a.log_file', sys.stderr)		# redirect std err, if necessary

# now it works
print('print something')
```

### 日志输出到控制台，并写入文件中
```
import logging
 
class Logger(object):
 
    def __init__(self, log_file_name, log_level, logger_name):
 
        #创建一个logger
        self.__logger = logging.getLogger(logger_name)
 
        #指定日志的最低输出级别，默认为WARN级别
        self.__logger.setLevel(log_level)
 
        #创建一个handler用于写入日志文件
        file_handler = logging.FileHandler(log_file_name)
 
        #创建一个handler用于输出控制台
        console_handler = logging.StreamHandler()
 
        #定义handler的输出格式
        formatter = logging.Formatter('[%(asctime)s] - [logger name :%(name)s] - [%(filename)s file line:%(lineno)d] - %(levelname)s: %(message)s')
        file_handler.setFormatter(formatter)
        console_handler.setFormatter(formatter)
 
        # 给logger添加handler
        self.__logger.addHandler(file_handler)
        self.__logger.addHandler(console_handler)
 
 
    def get_log(self):
        return self.__logger
 
 
 
if __name__ == '__main__':
    logger = Logger(log_file_name='log.txt', log_level=logging.DEBUG, logger_name="test").get_log()
    logger.debug('testing ... ')
    logger.info('testing ... ')

```

python简单日志的使用
```python
logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
                    datefmt='%Y-%m-%d %H:%M:%S',
                    filename='log1.txt',
                    filemode='a')
                    
logging.basicConfig(
    level=logging.INFO,  # 控制台打印的日志级别
    filename=BASE_DIR + '/logs/app.log',  # 将日志写入log_new.log文件中
    filemode='w',  # 模式，有w和a，w就是写模式，每次都会重新写日志，覆盖之前的日志 a是追加模式，默认如果不写的话，就是追加模式
    format='%(asctime)s - %(pathname)s[line:%(lineno)d] - %(levelname)s: %(message)s'  # 日志格式
)
# logging.getLogger('apscheduler').setLevel(logging.WARN)

```

- 普通python 项目使用日志
```python
# 创建 log_a.py
import  logging
logging.basicConfig(level=logging.DEBUG,
                format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',
                datefmt='%a, %d %b %Y %H:%M:%S',
                filename='myapp.log',
                filemode='w')

logger = logging.getLogger(__name__)

if __name__ == '__main__':
    logger.info("this is a log ")
    
    
# log_b.py文件使用通用的log_a.py
form log_a import logger

if __name__ == '__main__':
    logger.debug('this is debug message')
    logger.warning('i am warning message')
```