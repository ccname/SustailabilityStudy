
- https://regexr.com/ 正则在线网站
- .(点)  任意一个字符
- \w 字母数字下划线
- \W 除了字母数字下划线
- \d  \d+  \d{3} 以为或多位数字
- \D  非数字
- \s  空白字符
- \S  除了空白字符
- [\u4e00-\u9fa5]  匹配中文
- \+ 重复一次或多次
- [!\.~]\*  0次或多次
- ? 0次或一次
- {2} 匹配两次 
- {1,} 一次或多次 
- {0, } 0次或多次
- {0, 1} 0次或一次
- 1(?:37|38|82|83)\d{4}(\d{4}) 匹配移动号码，后四位相当于变量可以拿出来 (?: 匹配但不捕获)
- (1\d+)(?=元) 匹配以元结尾但不包含元
- (1\d+)(?!元|\d)  匹配不以元结尾
- (?<=¥)(1\d+)  反向预查 开头以¥开头但不包含¥
- (?<!¥)(\d+)   反向预查 开头不以¥开头但不包含¥

```python
match  # 从开头位置匹配,只匹配一次,可继续调用group和span方法获取相应结果
search  # 从任意位置匹配，匹配一次,可继续调用group和span方法获取相应结果
findall  # 匹配全部，返回一个列表，匹配不到返回空列表
finditer  # 返回一个迭代器类型，其中每个迭代元素是一个match对象，可继续调用group和span方法获取相应结果

# sub/subn 替换
import re
text = 'today is 2020-03-05'
print(re.sub('-', '', text)) #'today is 20200305'
print(re.sub('-', '', text, 1)) #'today is 202003-05'
print(re.sub('(\d{4})-(\d{2})-(\d{2})', r'\2/\3/\1', text))

# re.sub的一个变形方法是re.subn，区别是返回一个2元素的元组，其中第一个元素为替换结果，第二个为替换次数
import re
text = 'today is 2020-03-05'
print(re.subn('-', '', text)) #('today is 20200305', 2)

# split 切割函数
text = 'today is a re test, what do you mind?'
print(re.split(',', text)) #['today is a re test', ' what do you mind?']
```