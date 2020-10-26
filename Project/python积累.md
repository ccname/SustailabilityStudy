### python 执行时所传的参数
例子：
``` python
# main.py
import sys
print(sys.argv())
python main.py 3 hello //['main.py', '3', 'hello']
# 执行main.py返回一个字符串列表
```
#### 修改环境变量使mac在命令行输入python默认使用python3的环境。
```
vim进入文件
vim .base_profile  

在文件最后加入
alias python=/usr/local/bin/python3
```

#### pip 安装包失败解决
- pip install grequests -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
