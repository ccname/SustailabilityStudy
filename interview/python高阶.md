
```python
# 数字列表转为类似于元组的字符串
n = [1, 5, 4, 6, 7, 8, 5, 0]
print(*n)
print(*n[:3])
print(tuple(n))
n1 = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
b = map(str, n)
c = ''.join(b)
print(c)
print(list(i for i in zip(n, n1)))
```
#### 代码行内debug
```
import pdb

def make_bread():
    pdb.set_trace()
    return "I don't have time"

print(make_bread())

c: 继续执行
w: 显示当前正在执行的代码行的上下文信息
a: 打印当前函数的参数列表
s: 执行当前代码行，并停在第一个能停的地方（相当于单步进入）
n: 继续执行到当前函数的下一行，或者当前行直接返回（单步跳过）
```

#### map filter redece 用法
```
a = map(lambda x: x+2, [1, 2, 3, 4])

b = filter(lambda x: x>0, range(-5, 5))

from functools import reduce
product = reduce( (lambda x, y: x * y), [1, 2, 3, 4] )

```
#### python生成斐波那契数列

```python
# 方法一
from functools import lru_cache
# 装饰器是使用缓存。
@lru_cache(maxsize=32)
def f(n):
    if n < 2:
        return n
    return f(n-1) + f(n-2)


print([f(i) for i in range(40)])

方法二
def f(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b


print(list(f(70)))
```
#### python静态方法和类方法
```
class myDate(object):
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    @staticmethod
    def format_date(str_date):
        y, m, d = str_date.split('-')
        return myDate(int(y), int(m), int(d))

    @classmethod
    def format_date2(cls, str_date):
        y, m, d = str_date.split('-')
        return cls(int(y), int(m), int(d))

    def yesterday(self):
        self.day -= 1

    def __str__(self):
        return f'今天是{self.year}年{self.month}月{self.day}日'


d1 = myDate.format_date2('2020-05-02')
d1.yesterday()
print(d1)
# 静态函数还有什么用处，类方法都能代替了！在做一些类的预处理，或者条件判断的时候，静态函数还是很有用的！
```
#### 类的继承
```
class Person(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def eat(self):
        print('i can eat')


class Teacher(Person):
    def __init__(self, name, age):
        super().__init__(name, age)
        self.gender = 'man'

    def eat(self):
        print('eee', self.name, self.age, self.gender)
        super().eat()


t = Teacher('whh', 22)
t.eat()
```

#### python列表的sort,sorted
- sort 在原有的基础上排序，没有返回值
- sorded 有返回值，返回值是一个新的list
```python
a = [9, 4, 6, 2, 7, 1]
a.sort()
print(a)
b = [9, 4, 6, 2, 7, 1]
c = sorted(b)
print(c)
```

#### 不适用任何内置函数把 str '123' 转换成int 123
```
a = '123'
a_dict = {'1': 1, '2': 2, '3': 3}
res = 0
for i in a:
    res *= 10
    res += a_dict[i]
print(res, type(res))
```

#### map() filter() reduce() sorted() 的使用
```python
#这些函数都可以传另一个函数当作参数使用，经常配合lambda函数使用

from functools import reduce

my_list = [1, 3, 5, 7]

# 列表每个元素加一
res_map = list(map(lambda x: x+1, my_list))
print(res_map)

# 过滤出列表小于5的元素
res_filter = list(filter(lambda x: x < 5, my_list))
print(res_filter)

# 列表的求和
res_reduce = reduce(lambda x, y: x+y, my_list, 0)
print(res_reduce)

# 字典根据值排序
my_dict = {'a': 1, 'b': 3, 'c': 4, 'd': 2}
res = sorted(my_dict.items(), key=lambda item: item[1])
print(res)

```

#### python 复制、拷贝、深拷贝
```python
import copy
a = [1, 2, 3, 4, ['a', 'b']]
b = a
c = copy.copy(a)
d = copy.deepcopy(a)

a.append(5)
a[4].append('c')

print('a=', a)
print('b=', b)
print('c=', c)
print('d=', d)
```

#### 字典推导式, 将字符串 "k:1|k1:2|k2:3|k3:4" 处理成字典 {k:1,k1:2,...}
```python
# 特别注意最后的一个逗号。
# ValueError: not enough values to unpack (expected 2, got 1)
str1 = "k:1|k1:2|k2:3|k3:4"
res = {k: v for t in str1.split('|') for k, v in (t.split(':'),)}
print(res)
```

#### 列表字典排序
```python
from operator import itemgetter
rows = [
    {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
    {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
    {'fname': 'John', 'lname': 'Cleese', 'uid': 1001},
    {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
]

a = sorted(rows, key=lambda x: x['uid'])
print(a)

b = sorted(rows, key=itemgetter('lname', 'fname'))
print(b)

# c = min(rows, key=lambda x: x['uid'])
c = min(rows, key=itemgetter('uid'))
print(c)
```
#### 100以内的质数
```python
res = []
for i in range(2, 100):
    for j in range(2, i):
        if i % j == 0:
            break
    else:
        res.append(i)
print(res)
```
