---
title: Markdown入门
date: 2017-04-24 18:27:09
tags: Markdown
thumbnail: http://oowki3u7j.bkt.clouddn.com/timg.jpg
---
本文简单的介绍了Markdown的一些基本的语法。
# Markdown
Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。一直都有听说过Markdown，但是直到最近写博客时才真正的应用到，体会到他的方便性。

## 工具
我使用的是Atom，当然还有许多支持Markdown的工具。在Atom中建立一个.md为后缀的文件，这样的文件就是一个Markdown文件。ctrl+shift+m是开启Markdown预览的快捷键。

## Markdown语法

### 标题
>通过# 后面添加标题，#的个数代表标题的级数

``` bash
    # 标题一
    ## 标题二
    ### 标题三
```

### 引用
>通过>后面跟上文字，结束后要有空行

``` bash
>测试效果
```
效果：
>测试效果

### 字体
``` bash
*斜体* **粗体**
```
效果：
*斜体* **粗体**

### 链接
``` bash
[myblog](http://benzj.me/)
```
效果：
[myblog](http://benzj.me/)

### 图片
``` bash
![myblog](http://oowki3u7j.bkt.clouddn.com/timg.jpg)
```
效果:
![myblog](http://oowki3u7j.bkt.clouddn.com/timg.jpg)

### 分割线
``` bash
---
```
效果:<br>

### python中的转意字符
python 的转意字符为\

### 列表
```bash
无序列表
* 1
* 2
* 3

有序列表
1. 1
2. 2
3. 3

```
无序列表
* 1
* 2
* 3

有序列表

1. 1
2. 2
3. 3
