---
title: mac下安装ipython
date: 2017-05-28 16:47:35
tags: python
---
# mac下安装ipython
最精想好在电脑上安装ipython通过下属命令安装
```bash
pip install ipython
```
但是出错，报错如下
```bash
OSError: [Errno 1] Operation not permitted: '/tmp/pip-J0LWKY-uninstall/System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/six-1.4.1-py2.7.egg-info'
```
想想应该是权限问题那就加个sudo吧
```bash
sudo pip install ipython
```
不幸的是仍然出现这个问题，百度之后得知是由于mac系统开启了SIP保护

# 解决方案
1. 重启 Mac，按住 Command+R 键直到 Apple logo 出现
2. 进入 Recovery Mode点击 Utilities > Terminal
3. 在 Terminal 中输入 csrutil disable，之后回车
4. 重启 Mac

若是要启用sip的话同样方法输入csrutil enable
