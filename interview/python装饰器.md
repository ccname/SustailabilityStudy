```python

from flask import session, redirect, url_for

from functools import wraps

#带参数的装饰器
def json_output(p_state):
    def actual_decorator(func):
        @wraps(func)
        def inner(*args, **kwargs):
            print('===', args, kwargs) 
            print('+++', func()) 
            print(func.__name__)
            if str(p_state) == '0':
                session.clear()
                return func(*args, **kwargs)
            else:
                return func(*args, **kwargs)
        return inner
    return actual_decorator



# 不带参数的装饰器
def is_login(view_func):

    @wraps(view_func)
    def check_login(*args, **kwargs):
        # 验证登录
        if 'user_id' in session:
            return view_func(*args, **kwargs)
        else:
            # 验证失败
            return redirect(url_for('user.login'))
    return check_login


def foo(func):
    @wraps(func)
    def wapper(**args, **kwargs):
        return func(**args, **kwargs)
    return wapper


# 装饰器
from functools import wraps
def foo1(name='whh'):
    def foo(func):
        @wraps(func)
        def f1(*args, **kwargs):
            print('111', args)
            func()
            print('222', kwargs)
            if name == 'whh':
                return func(*args, **kwargs)
            else:
                print('you see see you')
        return f1
    return foo


@foo1(name='hh')
def f2():
    print('abcd')


f2()
```
#### @property装饰器
python类中的方法前加@property装饰器修饰，这个方法可以当作类的属性调用。
```
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self._score = score

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, new_val):
        self._score = new_val


student = Student('whh', 90)
print(student.name, student.score)
student.score = 99
print(student.name, student._score)
```


#### 写一个装饰器使得函数返回结果加粗和斜体
    @makebold
    @makeitalic
    def say():
       return "Hello"
    
    "<b><i>Hello</i></b>"
```python
from functools import wraps

# 加粗
def makebold(fn):
    @wraps(fn)
    def wrapped(*args, **kwargs):
        return '<b>' + fn(*args, **kwargs) + '</b>'
    return wrapped

# 加斜体
def makeitalic(fn):
    @wraps(fn)
    def wrapped(*args, **kwargs):
        return '<i>' + fn(*args, **kwargs) + '</i>'
    return wrapped


@makebold
@makeitalic
def say():
    return "Hello"


print(say())


@makebold
@makeitalic
def log(s):
    return s

# 传参数使值可变
print(log('beautiful'))
```