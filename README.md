# 我的个人博客
这个博客是机遇hexo来搭建的，如何在自己的电脑中运行

## 首先下载项目
```bash
git clone https://github.com/BENZJ/mblog.git
```

## 接着在电脑上init一个hexo工程
```bash
hexo init xxxx
# 这里的xxx可以随意天填写
```

## 复制文件
复制上面生成的文件中的node_modules文件夹和package.json,复制并替换元mblog下面的

## 运行
接着在mblog文件下运行
```bash
hexo s
```
如果出现 cannot Get/ 错误，请在目录下执行npm install

## 发布
```bash
npm install hexo-deployer-git --save
hexo d
```
