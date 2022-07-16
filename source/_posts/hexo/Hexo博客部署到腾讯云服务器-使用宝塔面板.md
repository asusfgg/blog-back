---
title: Hexo博客部署到腾讯云服务器(使用宝塔面板)
updated: 更新日期
cover: https://cdn.pixabay.com/photo/2019/06/17/19/48/source-4280758_960_720.jpg
description: Hexo博客部署到腾讯云服务器(使用宝塔面板)
abbrlink: d777d7aa
date: 2022-07-03 19:19:32
tags: hexo部署
swiper_index:
---

**使用宝塔面板的好处：**

- **操作更方便，习惯了Windows或者对Linux不熟悉的朋友可以更方便**
- **添加证书更加方便，而且可以开启HTTPS**

**我的服务器：**

- 系统 `宝塔Linux面板 7.9.2 腾讯云专享版`
- 配置 `通用型/2核/2GB/4Mbps`

**服务器需要的环境**

- 环境：`git`，`Nginx`，`宝塔Linux`
- 使用`git` 自动化部署发布

打开腾讯云，进入【云服务器】→【登录】



![img](https://pic4.zhimg.com/80/v2-e5ffd3d52a831fe8033d2134d841e55b_720w.jpg)



> 初始密码在右上角消息里面有

## 1】Git安装及配置

### 一、安装依赖库和编译工具

- 安装依赖库：

```
bash yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
```

然后会出现：

```bash
Is this ok [y/d/N]:
```

输入`y`继续安装，后面也一样。

- 安装编译工具：

```
bash yum install gcc perl-ExtUtils-MakeMaker package
```

### 二、下载 git并解压编译安装

- 查看服务器已有的git版本

```bash
git --version
```

然后会看到：

```bash
git version 1.8.3.1
```

> 但是官网版本已经更新了，因为yum仓库的Git版本更新的时间会存在延时，我们这里采用源码包安装方式安装。

- 将陈旧版本的git删除

```bash
yum remove git
```

- 选择一个目录来存放下载下来的 git 安装包。这里选择了`/usr/local/src` 目录

```
bash cd /usr/local/src
```

- 下载最新版git到`/usr/local/src`，可以在官网找到版本，目前最新版本是2.26.0。

```bash
wget http://ftp.ntu.edu.tw/software/scm/git/git-2.26.0.tar.gz
```

- 解压到当前目录

```bash
tar -zvxf git-2.26.0.tar.gz
```

- 进入 `git-2.26.0.tar.gz` 目录下

```bash
cd git-2.26.0
```

- 执行编译

```bash
make prefix=/usr/local/git all
```

- 安装 git 到 /usr/local/git 目录下

```bash
make prefix=/usr/local/git install
```

### 三、配置 git 环境变量

- 打开环境变量配置文件

```bash
vim /etc/profile
```

**按i进入编辑模式，按向下键到底部，添加下面两行命令：**

```bash
PATH=$PATH:/usr/local/git/bin   # git 的目录
export PATH
```

**按`esc`退出，按`:wq`保存编辑。(注意是先`:`再是`wq`)**



![img](https://pic1.zhimg.com/80/v2-b7b39976fb970f12779208d4e183d398_720w.jpg)



- 使 git 环境变量生效

```bash
source /etc/profile
```

- 验证安装完成，查看 git 的版本号

```bash
git --version
```

这时候我们的git版本已经变成了：

```bash
git version 2.26.0
```



![img](https://pic4.zhimg.com/80/v2-7f7e449da983aa95440085ee9b365cbf_720w.jpg)



### 四、创建 git 用户

- 创建git用户

```bash
adduser git
```

- 获取权限

```bash
chmod 740 /etc/sudoers
vim /etc/sudoers
```

按 `i` 键进入文件的编辑模式，按向下键找到如下字段

```bash
root    ALL=(ALL)       ALL
```

在其后面增加一句：

```bash
git     ALL=(ALL)       ALL
```

**按 `Esc` 键退出编辑模式，输入`:wq` 保存退出。（先输入`:`，然后输入`wq`回车）**

- 退回权限

```bash
chmod 400 /etc/sudoers
```

### 五、配置密钥

- 创建密钥

来到这里的小伙伴应该都已经有了自己的hexo博客，那么肯定已经创建过自己的密钥，一般存放在`c/用户/.ssh`下。



![img](https://pic3.zhimg.com/80/v2-84cf4e4e3f6304e272133ee33d3e56fe_720w.jpg)



如果没有自己的密钥，可以移步我之前的教程，里面有密钥创建步骤

[Github + Hexo 搭建个人博客超详细教程](https://link.zhihu.com/?target=https%3A//www.muyiio.com/2020/02/18/1/)

- 将密钥保存在服务器(之前有密钥的直接复制就可以)

将`id_rsa.pub`里面的密钥复制,在服务器运行下面命令，创建.ssh文件夹

```bash
su git
mkdir ~/.ssh
```

创建`.ssh/authorized_keys`文件，打开`authorized_keys`文件并将刚才在本地机器复制的内容拷贝其中并保存

```bash
vim ~/.ssh/authorized_keys
```

**按`i`进入编辑模式粘贴完按 `Esc` 键退出编辑模式，输入`:wq` 保存退出。（先输入`:`，然后输入`wq`回车）**

- 修改权限

```bash
chmod 755 ~
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

- 测试本地连接服务器

在本地电脑git bash here

```bash
//yourIp为远程服务器的ip地址
ssh -v git@yourIp     //yourIp为你的服务器ip
```

如图则证明本地机器与远程机器已经接通



![img](https://pic4.zhimg.com/80/v2-44add4f8630a0765a426c5956c82f1b3_720w.jpg)



### 六、创建git仓库

- 切换到root用户，创建一个目录用于存储网站的根目录

```bash
su root
```

- 创建网站的根目录

```bash
mkdir /home/hexo
```

- 给予权限

```bash
chown git:git -R /home/hexo
```

### 七、自动化部署

- 获取root权限

```bash
su root
```

- 建立git仓库

```bash
cd /home/git
git init --bare blog.git
```

- 修改blog.git权限

```bash
chown git:git -R blog.git
```

- 在 `/home/hexo/blog.git` 下，有一个自动生成的 `hooks` 文件夹，我们创建一个新的 `git` 钩子 `post-receive`，用于自动部署。

```bash
vim blog.git/hooks/post-receive
```

- **按 `i` 键进入文件的编辑模式**，在该文件中添加两行代码（将下边的代码粘贴进去)，指定 Git 的工作树（源代码）和 Git 目录

```bash
#!/bin/bash 
 git --work-tree=/home/hexo --git-dir=/home/git/blog.git checkout -f
```

**按 `Esc` 键退出编辑模式，输入`:wq` 保存退出。（先输入`：`，然后输入`wq`回车）**



![img](https://pic2.zhimg.com/80/v2-b7d46437ae28cb6d13712accaf0b81f5_720w.jpg)



- 修改文件权限，使得其可执行。

```bash
chmod +x /home/git/blog.git/hooks/post-receive
```

## 2】安装宝塔

> 宝塔Linux面板是提升运维效率的服务器管理软件，支持一键LAMP/LNMP/集群/监控/网站/FTP/数据库/JAVA等100多项服务器管理功能。支持的操作系统有CentOS，Ubuntu、Debian、Fedora.



![img](https://pic2.zhimg.com/80/v2-da314cc6727ff0a8d316ee52939e1d31_720w.jpg)



- 首先切换到根目录

```bash
cd ~
```

- **安装6.9稳定版（宝塔linux6.x版本基于centos7开发，适用于centos7.x版本）**

```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```

**中间会让你输入`y/n`继续安装，输入`y`**

- **7.x版本以下安装宝塔5.9**

```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
```

## 3】登录宝塔面板

- **宝塔安装完成后会出现以下字样**

```nginx
================================================================

BT-Panel default info!
==================================================================

Bt-Panel-URL: http://119.34.25.52:8888/af293c8c
username: aaaaaaaaa
password: 123456789
Warning:
If you cannot access the panel, 

release the following port (8888|888|80|443|20|21) in the security group
==================================================================
```

**注明：**

```nginx
Bt-Panel-URL: http://119.34.25.52:8888/af293c8c       #登录宝塔面板的地址
username: aaaaaaaaa                                   #账号
password: 123456789                                   #密码
```

然后我们来到浏览器输入URL进入宝塔面板，再输入账号密码登录。

- **不知道账号密码以及登录地址在服务器输入下面命令即可**

```nginx
/etc/init.d/bt default
```

- **安装LNMP（推荐）**



![img](https://pic3.zhimg.com/80/v2-30bc2977e2ca834e4269a1f5aa0c17a6_720w.jpg)



等待安装完成。

## 4】添加网站

- **点击【网站】→【添加站点】**



![img](https://pic2.zhimg.com/80/v2-cc75fb2e340bafafa55e90dd92ba0935_720w.jpg)



- **添加密钥和证书并强制Https**

> **如果没有申请证书就去域名注册商处免费申请一个，这里我以腾讯云为例**

1.首先下载证书：



![img](https://pic1.zhimg.com/80/v2-801a2fcdff726c73eb24eb0ffbbf3674_720w.jpg)



2.在本地解压打开，我们可以看到证书目录文件：



![img](https://pic4.zhimg.com/80/v2-685ae67a491ded22acc94c1705335537_720w.jpg)



3.宝塔面板只需要用到`Nginx`或`Apache`的，其他无需理会。

**①使用Nginx:**



![img](https://pic3.zhimg.com/80/v2-91c78434791ae612c4bac155cc817af6_720w.jpg)



其中`.key`后缀的是服务器私钥，填入面板证书的左边框中（用文本编辑器完整复制粘贴进去）

`.crt`后缀的是证书（也可能是pem后缀），填入面板证书的右边框中（用文本编辑器完整复制粘贴进去）

**②使用Apache:**



![img](https://pic4.zhimg.com/80/v2-6f12dd5da90d75fbc28a786d3744c04b_720w.jpg)



其中`.key`后缀的是服务器私钥，填入面板证书的左边框中（用文本编辑器完整复制粘贴进去）

1、2两张证书则需要合并填入面板证书的右边蓝框中（用文本编辑器完整复制粘贴进去）

**若不合并只填蓝框域名证书手机访问就会报缺失证书链/不安全等同时**

**若顺序不正确会导致apache无法正常启动**

4.证书添加成功后在右上角强制Https



![img](https://pic1.zhimg.com/80/v2-ea7612c59f1aa3f25bcffcc22f4d400c_720w.jpg)



## 5】配置本地Hexo

- 博客根目录_config下增加

```text
deploy:
    type: git
    repo: root@***(服务器ip,内网外网都行):/home/git/blog.git    #仓库地址
    branch: master    #分支
```

- 部署

```text
hexo clean
hexo g
hexo d
```

- 输入`hexo d`的时候，会要求你输入自己的服务器密码

```text
Branch 'master' set up to track remote branch 'master' from 'https://e.coding.net/godxiaolon/godxiaolon.git'.
On branch master
nothing to commit, working tree clean
root@119.25.56.82's password:
Enumerating objects: 182, done.
Counting objects: 100% (182/182), done.
Delta compression using up to 12 threads
Compressing objects: 100% (61/61), done.
Writing objects: 100% (95/95), 73.08 KiB | 3.18 MiB/s, done.
Total 95 (delta 45), reused 0 (delta 0)
remote: hooks/post-receive: line 1: t: command not found
To 118.25.27.52:/home/git/hexoBlog.git
   8df3691..7d63b39  HEAD -> master
Branch 'master' set up to track remote branch 'master' from 'root@118.25.27.52:/home/git/hexoBlog.git'.
INFO  Deploy done: git
```

> 输入密码不会有显示，输完回车就可以

- **如果出现`bash: git-receive-pack: command not found`,则运行：**

```text
sudo ln -s /usr/local/git/bin/git-receive-pack  /usr/bin/git-receive-pack
```



![img](https://pic3.zhimg.com/80/v2-f835fd310b1b361a250ba7a46a96e63e_720w.jpg)



- 访问服务器ip，看看有没有成功



![img](https://pic4.zhimg.com/80/v2-76bf143b2dd7ec1769fe3f62265cccc7_720w.jpg)



**可以看到，使用宝塔面板部署了证书，浏览器标识为安全。**

## 6】总结

宝塔的实用性挺广的，喜欢的小伙伴们可以去探索~

**教程有不对的地方欢迎指正~**