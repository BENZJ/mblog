---
title: 逆向工程入门之abexcreakme1
date: 2017-04-22 21:52:48
tags: 逆向工程
thumbnail: /img/逆向工程入门之abexcreakme1/head.png
---
## 关于学习逆向工程
最近开始学习逆向工程原理这本书。为了监督自己的自学所以决定在这里把自己的学习心得记录下来。为方便探讨和交流下面是本书的下面放上了百度云盘里。https://pan.baidu.com/s/1bo1EjyJ
 {% img img_wrap /img/逆向工程入门之abexcreakme1/bookcover.png %}  
### abexcm1小程序
abexcm1是本书中的逆向入门小程序。在下面的连接中可以下载（360会报毒），其运行界面入下图所示
 {% img  /img/逆向工程入门之abexcreakme1/stbox.png %}   {% img  /img/逆向工程入门之abexcreakme1/errorbox.png %}  
开始时后程序说让它认为你是个CD-ROM。点击后会弹出一个错误框，你不是CD-ROM。程序被Creak后会出现如下界面
{% img  /img/逆向工程入门之abexcreakme1/yeahbox.png %}  
### 调试工具OllyDbg
OllyDbg这是一款很流行的动态调试工具也是本书中采用的调试工具。快捷键如下所示
  1.F9+ctrl调到下个函数开始前
  2.F2+ctrl重新开始运行
  3.F7单步运行（step in）
  4.F8单步运行（step over）
## 程序调试
### 启动
通过Ollydgb打开abexcm1.exe界面如下。此时的界面左侧的地址栏都是以777位开头的就我个人通过近段时间的调试，从经验的出7开头的通常是系统调用里的函数。
{% img  /img/逆向工程入门之abexcreakme1/stolly.png %}  跳转出这一部分，按F9+Ctrl跳转直至出现如下片段通过观察左侧的地址栏为00开头和刚才不通过观察此时已经执行到了弹出框主程序的部分。
{% img  /img/逆向工程入门之abexcreakme1/mainpage.png %}
### 代码分析
{% img  /img/逆向工程入门之abexcreakme1/code.png %}
### OllyDbg修改代码
将上图中的JE改为JMP，在汇编中JMP为无条件跳转。则就能实现破解

## 心得与体会
这是我写的第一篇博客还是有点小激动。刚刚开始学习逆向工程，只是能大体的看懂程序并未分析到细节，如有错误请指正。
