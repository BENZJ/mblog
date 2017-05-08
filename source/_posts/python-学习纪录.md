---
title: python 学习纪录
date: 2017-05-06 15:41:18
tags:
---
这篇博客用来纪录自己在python学习过程中的每日收获
# python语法习惯
## \_\_init\_\_.py

\_\_init\_\_.py，是要想让一个文件夹成为包的必须的文件！这个文件可以为空
>个人理解在调用同一个文件夹下其他.py文件的方法时用到

# python基本的语法
## python循环遍历
python的循环遍历方法主要有while循环和for循环两种方式
### while循环
```python
count = 0
while (count<10):
    print 'Now the count is:',count
    count=count+1
```

### for循环的几种方式
in [数组] 的方式
```python
lists = ['apple','banana','pear']
for fruit in lists:
    print 'Now fruit is:',fruit
```
in rang()的方式
```python
lists = ['apple','banana','pear']
for fruit in lists:
    print "The ",nowpoint," is ",lists[nowpoint]
```
通过for循环来遍历目录文件下所有jpg结尾的文件
```python
import os
def get_imlist(path):
    return [os.path.join(path,f) for f in os.listdir(path) if f.endswith('.jpg')]
```

## 调用系统命令
通过导入os包来实现,例如说调用清屏
```python
import os  
os.system('clear')  
```
