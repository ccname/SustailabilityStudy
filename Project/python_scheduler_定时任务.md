
参考链接 
- https://cloud.tencent.com/developer/article/1406495
- https://blog.csdn.net/qq_33961117/article/details/88990040

**schedulers（调度器）
我个人觉得 APScheduler 非常好用的原因。它提供 7 种调度器，能够满足我们各种场景的需要。例如：后台执行某个操作，异步执行操作等。调度器分别是**：

- **BlockingScheduler** : 调度器在当前进程的主线程中运行，也就是会阻塞当前线程。
- **BackgroundScheduler** : 调度器在后台线程中运行，不会阻塞当前线程。
- **AsyncIOScheduler** : 结合 asyncio 模块（一个异步框架）一起使用。
- **GeventScheduler** : 程序中使用 gevent（高性能的Python并发框架）作为IO模型，和 GeventExecutor 配合使用。
- **TornadoScheduler** : 程序中使用 Tornado（一个web框架）的IO模型，用 ioloop.add_timeout 完成定时唤醒。
- **TwistedScheduler** : 配合 TwistedExecutor，用 reactor.callLater 完成定时唤醒。
- **QtScheduler** : 你的应用是一个 Qt 应用，需使用QTimer完成定时唤醒。

**triggers（触发器）**
- date 触发器 
 date 是最基本的一种调度，作业任务只会执行一次。它表示特定的时间点触发。
    ```
    # 在 2017-12-13 时刻运行一次 job_func 方法
    scheduler .add_job(job_func, 'date', run_date=date(2017, 12, 13), args=['text'])
    # 在 2017-12-13 14:00:00 时刻运行一次 job_func 方法
    scheduler .add_job(job_func, 'date', run_date=datetime(2017, 12, 13, 14, 0, 0), args=['text'])
    # 在 2017-12-13 14:00:01 时刻运行一次 job_func 方法
    scheduler .add_job(job_func, 'date', run_date='2017-12-13 14:00:01', args=['text'])
    ```
- interval 触发器 固定时间间隔触发
    ```
    # 每隔两分钟执行一次 job_func 方法
    scheduler .add_job(job_func, 'interval', minutes=2)
    # 在 2017-12-13 14:00:01 ~ 2017-12-13 14:00:10 之间, 每隔两分钟执行一次 job_func 方法
    scheduler .add_job(job_func, 'interval', minutes=2, start_date='2017-12-13 14:00:01' , end_date='2017-12-13 14:00:10')
    ```
- cron 触发器 
 在特定时间周期性地触发，和Linux crontab格式兼容。它是功能最强大的触发器。 
    ```
    # 在每年 1-3、7-9 月份中的每个星期一、二中的 00:00, 01:00, 02:00 和 03:00 执行 job_func 任务
    scheduler .add_job(job_func, 'cron', month='1-3,7-9',day='0, tue', hour='0-3')
    ```
#### 作业存储(job store)
- 有两种方式添加 job
```
# 第一中方式通过add_job()方式来

import datetime
import time

from apscheduler.schedulers.asyncio import AsyncIOScheduler
from apscheduler.schedulers.background import BackgroundScheduler
from apscheduler.schedulers.blocking import BlockingScheduler


def timedTask():
    print(datetime.datetime.utcnow().strftime("%Y-%m-%d %H:%M:%S.%f")[:-3])


if __name__ == '__main__':
    # 创建后台执行的 schedulers
    scheduler = BlockingScheduler()
    # 添加调度任务

    # 添加date调度器
    scheduler.add_job(timedTask, 'date', run_date=datetime.date(2017, 12, 13), args=['text'])
    # 添加interval调度器，调度方法为 timedTask，触发器选择 interval(间隔性)，间隔时长为 2 秒
    scheduler.add_job(timedTask, 'interval', seconds=2)
    # 添加cron调度器，在每年 1-3、7-9 月份中的每个星期一、二中的 00:00, 01:00, 02:00 和 03:00 执行 job_func 任务
    scheduler.add_job(timedTask, 'cron', month='1-3,7-9', day='0, tue', hour='0-3')

    # 启动调度任务
    scheduler.start()



----------------
# 第二种方式通过 装饰器的形式来添加job
import datetime
from apscheduler.schedulers.background import BackgroundScheduler

@scheduler.scheduled_job(job_func, 'interval', minutes=2)
def job_func(text):
    print(datetime.datetime.utcnow().strftime("%Y-%m-%d %H:%M:%S.%f")[:-3])

scheduler = BackgroundScheduler()
scheduler.start()
```

固定时间执行某任务
``` python
import datetime

import time


def doSth():
    print('test')
    # 假装做这件事情需要一分钟
    time.sleep(60)


def main(h=13, m=37):
    '''h表示设定的小时，m为设定的分钟'''
    while True:
        # 判断是否达到设定时间，例如0:00
        while True:
            now = datetime.datetime.now()
            # 到达设定时间，结束内循环
            if now.hour == h and now.minute == m:
                break
            # 不到时间就等20秒之后再次检测
            time.sleep(20)

        # 做正事，一天做一次
        doSth()


main()
```