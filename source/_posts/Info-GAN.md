---
title: Info GAN
date: 2017-12-19 10:47:10
tags:
- 机器学习
- Gan
categories: 机器学习
mathJax: true
---
<script ty-e="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
# Info GAN
Info GAN的提出也是和之前的CGAN(条件GAN)目的是一样的，都是为了解决原始的GAN通过随机噪声Z来生成的图像是我们不可控的。但是他们之间的不同点是，CGAN是有监督的需要事先打好标签的，Info GAN是无监督的。
## 论文部分翻译
>In this paper, rather than using a single unstructured noise vector, we propose to decompose the input noise vector into two parts: (i) z, which is treated as source of incompressible noise; (ii) c, which we will call the latent code and will target the salient structured semantic features of the data distribution.

这句说明了他把用于生成的噪声分成了两个部分,(i)z,被认为是一个不可压缩的噪声来源（先埋坑我也不懂）,（ii）c用来表示语意上的特征

>To cope with the problem of trivial codes, we propose an information-theoretic regularization: there should be high mutual information between latent codes c and generator distribution G(z, c). Thus I(c; G(z, c)) should be high.

作者提出了一个信息论正则化(不知道是不是这么翻译):认为隐变元  \\(c\\)  应该和生成的图像   \\( G(z,c)\\) 之间应该有很高的互信息(mutual information)互信息公式如下：
$$I(X;Y)=\sum_{x\in X}\sum_{y\in Y}p(x,y)log\frac{p(x,y)}{p(x)p(y)}$$

##论文核心
### 信息墒公式
$$
H(X) = \sum_i P(x_i)\log_b P(x_i)
$$
### 互信息公式
和上一节中提到的互信息公式经过推到之后应该是一样的)
$$
I(X;Y)=H(X)-H(X|Y)=H(Y)-H(Y|X)
$$
### 原始的GAN
$$\min\limits_{G}\max\limits_{D}V(G,D) = \mathbb{E}_{x\sim p\_data(x)}[\log D(x)] + \mathbb{E}_{z\sim p\_data(z)}[\log (1-D(x))]$$

### Info GAN损失公式
引入了互信息(mutual information)变成了：
$$\min\limits_{G}\max\limits_{D}V_I(G,D) = V(D,G)-\lambda I(c,G(z,c))$$
但是   \\( I(c,G(z,c))\\)  我们很难求它的最大值，所以引入了变分互信息最大化(Variational Mutual Information Maximization)求得它的下界

最后Info GAN变为了：

$$\min\limits_{G,Q}\max\limits_{D}V_{InfoGAN}(G,D) = V(D,G)-\lambda L_I(G,Q)$$

其实做的事就是把噪声分成了两部分，认为一部分代表语言，用了信息论中的互信息，损失里加入了c和  \\(G(z,c)\\)  的互信息

## MNIST tensorflow 代码

## 参考
信息论 http://blog.csdn.net/pipisorry/article/details/51695283
