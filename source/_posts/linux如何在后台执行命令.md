---
title: linux命令后台执行
date: 2017-05-28 16:47:35
tags: linux
---
# Linux 将命令放在后台执行
最近在用docker使用notebook时候，想要将ipython notebook放入后台进行

## 方式一采用&
```bash
ipython notebook &
```
采用这种方式虽然能够在后台运行，但是notebook的后台运行信息仍然会在终端中显示出来

## 方式二 采用 nohup 与&方式
```bash
# nohup ipython notebook &
```
采用nohup会在后台一直运行即使关闭当前终端，关闭进程的话可以使用ps命令查看进程号，然后kill命令关闭进程。
