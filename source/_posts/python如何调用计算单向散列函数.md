---
title: python如何调用计算单向散列函数
date: 2017-06-22 22:44:52
tags:
categories: 应用
---
# python如何调用计算单向散列函数
在密码学中涉及到许多种单向散列函数的应用。单向散列函数有很多种，比如MD5,SHA1,SHA256等等。在python中内带了hashlib库可以用来计算文件或者是字符串的散列值。

## hashlib的用法
```python
import hashlib
m = hashlib.sha256()
m.updata(src)
print print m.hexdigest()
```
### 计算文件的md5值
```python
# coding: utf-8

# In[2]:


import hashlib


# In[6]:


src = open("key_gen.ipynb","rb")


# In[7]:


md = hashlib.md5()


# In[10]:


buf = src.read()
md.update(buf)


# In[11]:


print md.hexdigest()


# In[ ]:
```
