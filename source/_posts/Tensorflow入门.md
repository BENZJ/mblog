---
title: Tensorflow入门
date: 2017-05-28 14:52:06
tags:
---
# Tensorflow 入门

## tensor张量
张量主要有3个属性，分别为名字（name），维度（shape），和类型（type）
```bash
In [1]: import tensorflow as tf

In [2]: a = tf.constant([1.0,2.0,3,0],name = 'a')

In [3]: print a
Tensor("a:0", shape=(4,), dtype=float32)
```

## 会话
开启回话主要有两种方法，还不太清楚Session()和InteractiveSession()有什么区别
### Session（）
```python
#开启session后去要关闭才行
sess = tf.Session()

result.eval(session=sess)
print result

sess.close()
```

```python
# 也可以使用with方法
with tf.Session() as sess:
    print(sess.run(result))
```
### InteractiveSession（）
```python
sess = tf.InteractiveSession ()
print (result.eval())
sess.close()
```
### 两种方法的不同点
* 使用InteractiveSession一个主要的变化是：运行在没有指定会话对象的情况下运行变量。这是与Session（）最大的不同。
* Session（）使用with..as..后可以不使用close关闭对话，而调用InteractiveSession需要在最后调用close
