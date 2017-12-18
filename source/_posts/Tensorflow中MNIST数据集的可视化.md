---
title: Tensorflow中MNIST数据集的可视化
date: 2017-09-19 14:45:33
tags:
categories: 机器学习
---
# Tensorflow中MNIST数据集的可视化
在学习Tensorflow深度学习的过程中首先学习的例子就是MNIST数据集,但是在tf中观察他就是一个纯数组.不能给人直观的理解,所以我想将这些数据抓换成图片输出看一下

## 代码的最终效果
![](http://oowki3u7j.bkt.clouddn.com/Tensorflow%EF%BC%AD%EF%BC%AE%EF%BC%A9%EF%BC%B3%EF%BC%B4%EF%BD%93%EF%BD%88%EF%BD%8F%EF%BD%95%EF%BD%94%EF%BD%83%EF%BD%81%EF%BD%94.png)
## 代码分析
### 导入需要用到的包
```python
#coding=utf-8
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data    #用于加载ＭＮＩＳＴ数据集
import matplotlib.pyplot as plt                               #用来在ipython中打印图像
import matplotlib.image as mpimg                              #读取图片
from PIL import Image                                         #用来生成和处理图片
import os                                                     #删除中间过程产生的文件
```
### 下载和生成数据集
```python
mnist = input_data.read_data_sets("MNIST_data/",one_hot=True)
```
### 找到one_hot对应的值
由于one_hot编码只有一个对位位置的值被置为１其他都是０说以找到为１的值就返回
```python
def one_hotshow(lable):
    for i in range(10):
        if lable[i]==1:
            return i
```
### 生成图像和打印数据
```python
def savimage(mnistdata):
    image1 = mnistdata.reshape([28,28])#首先把一维的数据还原回２８＊２８大小的矩阵
    im= Image.new("1", (28, 28))#生成一个２８＊２８大小的新的图片，第一个参数为图片的形式，１代表是黑白图片，后面这个元祖代表图片的大小
    for i in range(28):#遍历图片把图片修改每个点像素的值
        for j in range(28):
            im.putpixel((j,i),(255*image1[i,j],))#MNIST数据集合应该修图片的像素值预处理过，都是小于１的数值所以这里我们把原来的值乘以最大值２５５
    im.save('num1.jpg')#由于在ipython中PIL不能直接输出所以我们先存下然后再输出
    lena = mpimg.imread('num1.jpg')
    plt.imshow(lena) # 显示图片
    plt.axis('off')# 不显示坐标
    plt.show()#图片显示
    os.remove('num1.jpg')#打印出图片后就删除保存下的文件
```
### 调用函数
```python
def testMNist(index):
    print one_hotshow(mnist.train.labels[index])　#输出图片对应的数字标签值
    image1 =mnist.train.images[index]#获取ｍｎｉｓｔ数据集数据
    savimage(image1)
```
### 主函数
打印出前100张ＭＮＩＳＴ对应的图片
```python
for i in range(100):
    testMNist(i)
```
