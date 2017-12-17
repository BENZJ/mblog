---
title: GAN生成对抗网络
date: 2017-12-17 09:44:09
tags: 机器学习
thumbnail: http://oowki3u7j.bkt.clouddn.com/Ganimg.png
---

<script ty-e="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
# GAN生成对抗网络

## 生成对抗网络简介
[论文地址](http://www.baidu.com)这篇论文是Ian Goodfellow 在2014年提出的他他最早的提出了生成对抗网络的概念。是一个零和的博弈游戏，里面由两个网络，生成网络和对抗网络，生成网络要做的事情就是尽可能的欺骗对抗网络，对抗网络要做的就是判断数据是来自真实数据集还是生成网络所生成的数据，这两个网络相互对抗，一起进步学习，直到对抗网络无法判断出生成数据和原始数据的差异才会达到平衡。

## 论文的难点
这篇论文中所提到的很多数学的公式，不知道是否是我还没有真正的理解其中的意思，在我查看网上代码的时候发现都没有用到其中的公式。由于作者提供的代码是Theano写的，学习需要一定的成本。所以我在网上查到了一段tensorflow写的minist数据集生成的代码，在下面解释一下加深下对GAN的理解

## GAN的基本模型
### 图片说明
![](http://oowki3u7j.bkt.clouddn.com/gan_inlerstate.png)
### 符号说明

$$z   表示随机噪声$$
$$G() 表示生成网络$$
$$D() 表示鉴别网络$$
$$x   表示真实的数据$$
$$G(z)用来表示通过噪声z生成的数据$$

## Tensorflow代码
### 生成模型
生成模型是一个二层的全连接输入是一个噪声z，输出是一个784(=32*32)纬度的mnist图片数据
```python
# 生成网络模型的定义
def generator(z):
    # GW1 100*128   Z None*100  G_b1 128
    G_h1 = tf.nn.relu(tf.matmul(z, G_W1) + G_b1)
    # G_h1 = None*128

    # G_h1 None*128 G_W2 128*784 G_b2 784
    G_log_prob = tf.matmul(G_h1, G_W2) + G_b2
    # G_log_prob = None*784
    G_prob = tf.nn.sigmoid(G_log_prob)

    return G_prob
```
### 对抗模型
对抗模型也是一个二层的全连接，输入x是一个tensor，x可以输入原始的数据(对应结构图中的左边鉴别网络)，也可以放入generator(z)生成的tensor（对应结构图中的右边的网络）
```python
# 对卡个网络模型的定义
def discriminator(x):
    # x None*784 D_W1 784*128
    D_h1 = tf.nn.relu(tf.matmul(x, D_W1) + D_b1)
    # D_h1 = None*128

    # D_h1 None*128 D_W2 128*1
    D_logit = tf.matmul(D_h1, D_W2) + D_b2
    # D_logit = None*1

    D_prob = tf.nn.sigmoid(D_logit)

    return D_prob, D_logit
```
### 网络的定义
D_real和D_fake分别对应结构图中的左边和右边
```python
# G_sample 表示的是一个生成好的图像，是一个tensor
G_sample = generator(Z)
D_real, D_logit_real = discriminator(X)
D_fake, D_logit_fake = discriminator(G_sample)
```
### 损失函数的定义
```python
# 损失函数
# tf.ones_like 用来生成全1的并且和形状和原来的tensor一样的东西
D_loss_real = \
    tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=D_logit_real, labels=tf.ones_like(D_logit_real)))
# D_loss_real表示鉴别网络把真实输入判错
D_loss_fake = \
    tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=D_logit_fake, labels=tf.zeros_like(D_logit_fake)))
# D_loss_fake表示把生成数据错判成真实数据
D_loss = D_loss_real + D_loss_fake

G_loss = \
    tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=D_logit_fake, labels=tf.ones_like(D_logit_fake)))
# 生成网络的想要自己生成的都判为1

```

### 生成图像的效果
![](https://wiseodd.github.io/img/2016-09-17-gan-tensorflow/training.gif)

### 代码地址
原作者代码 https://github.com/wiseodd/generative-models/blob/master/GAN/vanilla_gan/gan_tensorflow.py</br>
我修改后的的代码(主要加了注释还有修改了图片输出的代码)http://oowki3u7j.bkt.clouddn.com/Original_MNIST_Gan.zip
## 存在的问题
这份代码中并没有使用到论文中所提到的极大似然函数还有kl散度，要验证这个问题还是要去好好研究作者提供的theano代码才行


## 参考
https://wiseodd.github.io/techblog/2016/09/17/gan-tensorflow/
