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
