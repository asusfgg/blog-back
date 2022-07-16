---
title: 我的第一篇hexo文章
tags: hexo入门
cover: https://s3.bmp.ovh/imgs/2022/07/06/bc134af2f706c860.webp
abbrlink: 7d13b26d
date: 2022-06-21 20:02:52
---

## 第一章：Hello Hexo

## 第二章：相关指令

### npm指向淘宝源安装cnpm

### npm排错

npm.ini 改一个参数

### cnpm安装hexo

cnpm install -g hexo-cli      其中 -g全局安装

安装地址在：C:\Users\asusf\AppData\Roaming\npm\node_modules\hexo-cli

新建一个文件：mkdir fggblog

进入：cd fggblog

初始化：hexo init

查看文件列表：dir

启动博客：hexo s

本地地址：http://localhost:4000/

断开：Ctrl+c

博客内容路径：fggblog\source\\_posts

 创建博客：hexo n "我的第一篇hexo文章"

路径返回：cd ../.. 返回两层的意思

清理： hexo clean

生成： hexo g



## 第三章：备用记录

vim安装： npm i vim -g

### 远端部署

github新建仓库

仓库名称为 asusfgg.github.io 固定格式

在C:\Users\asusf\AppData\Roaming\npm\node_modules\hexo-cli\fggblog目录下安装>cnpm install --save hexo-deployer-git

配置文件_config.yml

 文件尾部 修改：

`\# Deployment

\## Docs: https://hexo.io/docs/one-command-deployment

deploy:

 type: 'git'

 repo: 'https://github.com/asusfgg/asusfgg.github.io.git'

 branch: 'master'`

推送带远端：hexo d

### 换主题

note:一直是基于fggblog目录操作的

#### 安装

```git
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```

当然，也可以手撸：

git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia themes/yilia 后面这句themes/yilia 意思是往这个路径下塞。

#### 配置

修改hexo根目录下的 `_config.yml` ： `theme: yilia`

### 重新推送

先清理 hexo clean

在生成 hexo g

注意！

要本地启动一下

hexo s

然后再推送远端 hexo d







## 结语