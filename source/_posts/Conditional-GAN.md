---
title: Conditional GAN
date: 2017-12-18 17:46:54
tags: 机器学习
categories: 机器学习
thumbnail: http://oowki3u7j.bkt.clouddn.com/Ganimg.png
---
<script ty-e="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>



# Conditional GAN(条件GAN)
Conditional GAN是在GAN的基础上提出的，普通的GAN只能随机的生成图像，我们很难通过随机的噪声来控制生成我们想要的图像。所以在这样的基础上提出了条件GAN。原始的[论文地址](https://arxiv.org/abs/1411.1784)，若还不是很清楚GAN的具体原理，可以参考[上一篇博客](http://benzj.me/2017/12/17/GAN%E7%94%9F%E6%88%90%E5%AF%B9%E6%8A%97%E7%BD%91%E7%BB%9C/).下面还是和上一篇一样用mnist的例子来解释条件GAN

## Conditional GAN
### 公式说明

#### 普通GAN的结构公式


$$\min\limits_{G}\max\limits_{D} = \mathbb{E}_{x\sim p\_data(x)}[\log D(x)] + \mathbb{E}_{z\sim p\_data(z)}[\log (1-D(x))]$$

#### Conditional GAN公式结构


$$\min\limits_{G}\max\limits_{D} = \mathbb{E}_{x\sim p\_data(x)}[\log D(x|y)] + \mathbb{E}_{z\sim p\_data(z)}[\log (1-D(x|y))]$$

从公式上我们大致可以这样的理解就是在生成图像和鉴别图像的时后都加上了一个相同的y作为条件，所以称这种GAN为条件GAN
### 结构图
![c_gan](http://oowki3u7j.bkt.clouddn.com/c_gan.png)

### MNIST代码
#### 生成网络
```python
def generator(z, y):
    # z 16*100  y 16*10
    inputs = tf.concat(axis=1, values=[z, y])
    # input 16 * 110

    # G_W1 110*128
    G_h1 = tf.nn.relu(tf.matmul(inputs, G_W1) + G_b1)
    # G_h1 16*128

    # G_W2 128*784
    G_log_prob = tf.matmul(G_h1, G_W2) + G_b2
    # G_log_prob 16*784

    G_prob = tf.nn.sigmoid(G_log_prob)
    # 返回的是图片
    return G_prob
```
#### 对抗网络
```python
def discriminator(x, y):
    # 把下x,y合并成一个tensor
    # x = 64*784    y = 64*10
    inputs = tf.concat(axis=1, values=[x, y])
    # inputs = 64*794

    # D_W1 794*128
    D_h1 = tf.nn.relu(tf.matmul(inputs, D_W1) + D_b1)
    # D_h1 = 64*128

    # D_W2 128*1
    D_logit = tf.matmul(D_h1, D_W2) + D_b2
    # D_logit 64*1

    D_prob = tf.nn.sigmoid(D_logit)

    return D_prob, D_logit
```
这两个生成网络和对抗的网络和原先唯一的区别就在与输入变成了两个，并且在代码中可以看出第一句tf.concat把这两个tensor合并成了一个。
#### 损失函数
损失函数和之前的普通GAN是一样的
```python
D_loss_real = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=D_logit_real, labels=tf.ones_like(D_logit_real)))
D_loss_fake = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=D_logit_fake, labels=tf.zeros_like(D_logit_fake)))
D_loss = D_loss_real + D_loss_fake
G_loss = tf.reduce_mean(tf.nn.sigmoid_cross_entropy_with_logits(logits=D_logit_fake, labels=tf.ones_like(D_logit_fake)))
```
#### 生成图像
```python
Z_sample = sample_Z(n_sample, Z_dim)
# print Z_sample.shape
y_sample = np.zeros(shape=[n_sample, y_dim])
'''添加上条件用来控制我们想要生成的数字'''
y_sample[:, 5] = 1

# 生成好的图像
# y 是添加上的条件
# y_sample 16*10   z_sample 16*100
samples = sess.run(G_sample, feed_dict={Z: Z_sample, y:y_sample})
```

#### 实现效果
![gif](http://oowki3u7j.bkt.clouddn.com/c_gan.gif)
#### 代码地址
我的代码地址 http://oowki3u7j.bkt.clouddn.com/conditional_gan.zip

原始作者代码地址 https://github.com/wiseodd/generative-models/blob/master/GAN/conditional_gan/cgan_tensorflow.py

## 总结
在这个实验中在生成和对抗网络训练的时候都分别加入了他们呢对应的标签当作条件GAN中的条件y。所以我们在最后生成图像的时候比如我们想要得到的是5我们就把y设置为5，这样到最后生成图像会全部都是5.
## 参考
原作者博客 https://wiseodd.github.io/techblog/2016/12/24/conditional-gan-tensorflow/

markdown公式问题
http://2wildkids.com/2016/10/06/%E5%A6%82%E4%BD%95%E5%A4%84%E7%90%86Hexo%E5%92%8CMathJax%E7%9A%84%E5%85%BC%E5%AE%B9%E9%97%AE%E9%A2%98/
