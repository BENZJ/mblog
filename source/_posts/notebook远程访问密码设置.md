---
title: notebook远程访问密码设置
date: 2017-05-28 16:47:35
tags:
categories: 应用
---
# notebook远程访问密码设置

## 生成密钥
在python命令行中运行如下代码：
```python
>>> from notebook.auth import passwd
>>> passwd()
Enter password:
Verify password:
'sha1:2ebc88920dd4:.....这一串是你的密钥复制下来'
>>>
```

## 设置密码

```bash
vi ~/.jupyter/jupyter_notebook_config.py
```
```python
import os
from IPython.lib import passwd

c.NotebookApp.ip = '*'
c.NotebookApp.port = int(os.getenv('PORT', 8888))
c.NotebookApp.open_browser = False
c.MultiKernelManager.default_kernel_name = 'python2'



#添加密码
c.NotebookApp.password = u'sha:上面复制的密钥粘贴到这里'
```

## 重启notebook
```bash
ipython notebook
```
