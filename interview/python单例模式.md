**常见的单例模式**
- 比如，某个服务器程序的配置信息存放在一个文件中，客户端通过一个 AppConfig 的类来读取配置文件的信息。如果在程序运行期间，有很多地方都需要使用配置文件的内容，也就是说，很多地方都需要创建 AppConfig 对象的实例，这就导致系统中存在多个 AppConfig 的实例对象，而这样会严重浪费内存资源，尤其是在配置文件内容很多的情况下。事实上，类似 AppConfig 这样的类，我们希望在程序运行期间只存在一个实例对象。

- 当有同步需要的时候，可以通过一个实例来进行同步控制，比如对某个共享文件（如日志文件）的控制，对计数器的同步控制等，这种情况下由于只有一个实例，所以不用担心同步问题。

- Python的logger就是一个单例模式，用以日志记录；Windows的资源管理器是一个单例模式；线程池，数据库连接池等资源池一般也用单例模式；网站计数器等等，这些都是单例模式。

**应用场景**

当每个实例都会占用资源，而且实例初始化会影响性能，这个时候就可以考虑使用单例模式，它给我们带来的好处是只有一个实例占用资源，并且只需初始化一次；归纳为以下几条：

- 要求生产唯一序列号。

- WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。

- 创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。
```python
import pymysql
import threading
from DBUtils.PooledDB import PooledDB

class SingletonDBPool(object):
    _instance_lock = threading.Lock()

    def __init__(self):
        self.pool = PooledDB(
            creator=pymysql,  ## 使用链接数据库的模块
            maxconnections=6,  ## 连接池允许的最大连接数，0和None表示不限制连接数
            mincached=2,  ## 初始化时，链接池中至少创建的空闲的链接，0表示不创建

            maxcached=5,  ## 链接池中最多闲置的链接，0和None不限制
            maxshared=3,
            ## 链接池中最多共享的链接数量，0和None表示全部共享。PS: 无用，因为pymysql和MySQLdb等模块的 threadsafety都为1，所有值无论设置为多少，_maxcached永远为0，所以永远是所有链接都共享。
            blocking=True,  ## 连接池中如果没有可用连接后，是否阻塞等待。True，等待；False，不等待然后报错
            maxusage=None,  ## 一个链接最多被重复使用的次数，None表示无限制
            setsession=[],  ## 开始会话前执行的命令列表。如：["set datestyle to ...", "set time zone ..."]
            ping=0,
            ## ping MySQL服务端，检查是否服务可用。## 如：0 = None = never, 1 = default = whenever it is requested, 2 = when a cursor is created, 4 = when a query is executed, 7 = always
            host='127.0.0.1',
            port=3306,
            user='root',
            password='123',
            database='pooldb',
            charset='utf8'
        )

    def __new__(cls, *args, **kwargs):
        if not hasattr(SingletonDBPool, "_instance"):
            with SingletonDBPool._instance_lock:
                if not hasattr(SingletonDBPool, "_instance"):
                    SingletonDBPool._instance = object.__new__(cls, *args, **kwargs)
        return SingletonDBPool._instance

    def connect(self):
        return self.pool.connection()
        
def run():
    pool = SingletonDBPool()
    conn = pool.connect()
    ## xxxxxx
    cursor = conn.cursor()
    cursor.execute("select * from td where id=%s", [5, ])
    result = cursor.fetchall()  ## 获取数据
    cursor.close()
    conn.close()

if __name__ == '__main__':
    run()
```


**python 单例模式简单的实现**
```python
# 使用装饰器实现单例模式
def singleton(cls):
    instance = {}

    def test(*args, **kwargs):
        # 如果没有cls这个类则创建，并将cls所创建的实例保存在字典中
        if cls not in instance:
            instance[cls] = cls(*args, **kwargs)
        return instance[cls]
    return test


@singleton
class Student(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age


a = Student('whh', 22)
b = Student('fah', 20)

print(a == b)
print(a is b)


# 使用__new__实现单例模式
class Singleton(object):
    instance = None

    def __new__(cls, *args, **kwargs):
        if cls.instance is None:
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance

    def __init__(self, name, age):
        self.name = name
        self.age = age


a = Singleton('whh', 22)
b = Singleton('fah', 20)

print(a == b)
print(a is b)
```

