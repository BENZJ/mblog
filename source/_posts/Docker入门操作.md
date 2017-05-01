---
title: Docker入门操作
date: 2017-04-30 10:55:40
tags:
---
# Docker容器的入门操作

最近在学习安装spark的时候，在windows下安装遇到了许多问题，在知乎上有人建议使用Docker容器来部署不要在windows下面用，开始了解Docker。

## Docker的安装

### Windows安装
如果是win10，Docker有客户端可以直接安装（注意和windows的版本号有关之前用的版本号过低一直不能安装）

### Ubuntu 14.04安装
通过安装命令就能实现安装
```bash
apt-get install docker.io
```

## Docker基本命令

### docker pull拉取Docker镜像
比如要从hub.docker.com中拉取一个hello-world的镜像可以用如下命令
```bash
docker pull hello-world
```
### docker images查看镜像
通过docker images命令我们可以查看我们已经下载的所有镜像

### docker run启动镜像并且创建容器
docker run 命令后面可以跟镜像的名字，或者是镜像的uuid值，uuid可以通过

### docker export打包导出容器

### docker import导入容器

## Docker常用方法

### 删除所有未启动的镜像
```bash
$ sudo docker rm $(docker ps -a -q)
```
### 用Dockerfile来创建新的镜像
