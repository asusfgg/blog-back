---
title: hexo发生error：spawn failed错误的解决方法
tags: hexo问题解决
cover: 'https://images.pexels.com/photos/1888015/pexels-photo-1888015.jpeg'
copyright: false
abbrlink: ca599235
date: 2022-06-22 21:04:36
---

# 项目场景：[hexo](https://so.csdn.net/so/search?q=hexo&spm=1001.2101.3001.7020)发生error：spawn failed错误的解决方法

------

# 问题描述

先是出现错误：

```bash
error：spawn failed...
```

然后经过一些博客的操作会出现以下问题：

```bash
fatal: cannot lock ref 'HEAD': unable to resolve reference HEAD: Invalid argument error: src refspec
```

或者：

```bash
error: src refspec HEAD does not match any.
```

下面图片是我自己`hexo d`上传[GitHub](https://so.csdn.net/so/search?q=GitHub&spm=1001.2101.3001.7020)以后出现的报错：

```c
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (D:\Program Files\blog\node_modules\hexo-util\lib\spawn.js:51:21)
      at ChildProcess.emit (node:events:526:28)
      at ChildProcess.cp.emit (D:\Program Files\blog\node_modules\cross-spawn\lib\enoent.js:34:29)
      at Process.ChildProcess._handle.onexit (node:internal/child_process:291:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html

```

------

# 原因分析：

> 问题大多是因为git进行push或者hexo d的时候改变了一些.deploy_git文件下的内容。

------

# 解决方案：

## 解决方法：一

1.删除 `.deploy_git` 文件夹;
2.输入 `git config --global core.autocrlf false`
3.然后，依次执行：

```handlebars
hexo clean
hexo g
hexo d
```

## 解决方法 ：二

1.进入hexo根目录
2.直接把箭头指向的`.deploy_git`和`public`直接删除！
![在这里插入图片描述](https://img-blog.csdnimg.cn/ffa9155ebfc543d48ef7eb909b6b4716.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bq45oqx,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)
3.然后，依次执行：

```handlebars
hexo clean
hexo g
hexo d
```

## 注意！！：删除这两个文件夹不会造成影响！执行完hexo三连会重新部署 简单暴力！