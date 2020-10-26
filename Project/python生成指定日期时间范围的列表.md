```python
import random
import time
from datetime import datetime


def strTimeProp(start, end, prop, frmt):
    stime = time.mktime(time.strptime(start, frmt))
    etime = time.mktime(time.strptime(end, frmt))
    ptime = stime + prop * (etime - stime)
    return int(ptime)


def randomTimestamp(start, end, frmt='%Y-%m-%d %H:%M:%S'):
    return strTimeProp(start, end, random.random(), frmt)


def randomDate(start, end, frmt='%Y-%m-%d %H:%M:%S'):
    return time.strftime(frmt, time.localtime(strTimeProp(start, end, random.random(), frmt)))


def randomTimestampList(start, end, n, frmt='%Y-%m-%d %H:%M:%S'):
    return [randomTimestamp(start, end, frmt) for _ in range(n)]


def randomDateList(start, end, n, frmt='%Y-%m-%d %H:%M:%S'):
    return [randomDate(start, end, frmt) for _ in range(n)]


start = '2020-04-26 12:12:12'
end = '2020-05-01 00:00:00'
lenth = 1010
# print(randomTimestamp(start, end))
# print(randomDate(start,end))
# print(randomTimestampList(start, end, lenth))
print(randomDateList(start, end, lenth))

```