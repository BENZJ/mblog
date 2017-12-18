---
title: NASM汇编
date: 2017-05-24 16:39:25
tags:
categories: 应用
---
# SASM安装教程
[SASM](http://dman95.github.io/SASM/english.html)是一个汇编的IDE
## 下载
首相从github中[下载](https://github.com/Dman95/SASM)

## 安装依赖
```bash
sudo apt install nasm qt4-qmake libqt4-dev libxcb1 libxcb-render0 libxcb-icccm4
```

## 安装
进入sasm的目录下执行以下命令
```bash
qmake
make
make install
```
中间会有些warning没去管他。安装好后命令行输入sasm或者在应用中打开就会出现如下界面SASM就安装好了
![mainpng](http://oowki3u7j.bkt.clouddn.com/Screenshot%20from%202017-05-17%2011.16.09.png)
## 问题以及解决
我在使用时候发现运行时出错：gcc: error: /tmp/SASM/macro.o: No such file or directory
![errorpng](http://oowki3u7j.bkt.clouddn.com/Screenshot%20from%202017-05-17%2011.15.19.png
下载文件)
解决办法如下：
```bash
sudo apt install gcc-multilib
```
