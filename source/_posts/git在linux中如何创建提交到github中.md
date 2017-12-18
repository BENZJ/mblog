---
title: git在linux中如何创建提交到github中
date: 2017-07-06 21:23:51
tags:
categories: 应用
---
# 如何在linux下如何创建并且提交代码到github
1. 首先在github的网站上创建一个新的仓库复制仓库的链接例如： my.git
```bash
#创建一个git仓库
git init
```
2. 在命令中设置git的账号
```bash
git config --global user.name "You Name"
git config --global user.email yourmail@server.com
```

3. 设置账号git仓库的源
```bash
git remote add origin [此处写你的仓库地址]
```

4. 提交代码
```bash
git push -u origin master
```
# 参考
http://blog.csdn.net/a695017449/article/details/26103761
