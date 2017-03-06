+++
tags = ['tools', 'Ubuntu']
categories = ["Tools"]
date = "2017-02-27T13:55:22+08:00"
description = "记录在Ubuntu14.04系统下面配置Hugo的过程，包括安装Hugo，markdown工具remarkanle 以及配置搜狗输入法，github等。"
title = "Hugo在Ubuntu14.04系统下的配置 "

+++

### 安装Hugo

Ubuntu14.04 下面直接用apt-get安装貌似不行，参考[博主](https://vexxhost.com/resources/tutorials/how-to-install-and-use-hugo-the-static-site-generator-on-ubuntu-14-04/),直接下载源文件安装：
```
wget https://github.com/spf13/hugo/releases/download/v0.15/hugo_0.15_amd64.deb

sudo dpkg -i hugo_0.15_amd64.deb
```
即可，测试一下版本`hugo version`，成功输出版本号即安装成功。


### 安装Markdown编辑工具（remarkable）
- 从[此篇文章](http://locez.com/translation/best-markdown-editors-for-linux/)了解到 Remarkable，貌似apt-get也木有直接安装的，于是在[官网](https://remarkableapp.github.io/linux/download.html)下载.deb文件；
- 按照Hugo类似的命令`sudo dpkg -i ****.deb`安装之。
- 可以在终端直接用命令`remarkable` 打开.md文件

### 安装搜狗输入法for Linux
- [搜狗输入法官网](http://pinyin.sogou.com/linux/help.php)下载安装文件；
- 双击直接跳出更新管理器安装之；
- 安装后，需要在语言支持里面，keyboard input method system改为fcitx，此**步骤重要**；
- 重启，输入法生效。

### 配置github
由于hugo文件在另一台电脑已经配置好config.toml文件，所以只需要将整个文件夹copy过来，配置好Ubuntu本地的github即可
```
git init
git add -A
git commit -m "first commit"
git remote add origin git@github.com:tmhm/blog_well.git
git push -u origin master
```

### Hugo 命令Note
```
本地服务器，看样式：
hugo server --baseURL=http://127.0.0.1:1313/ -t hyde-y -D --bind="0.0.0.0"

在myblog根目录执行，生成静态网页，生成文件放在public文件夹：  

hugo -t hyde-y --baseURL "https://tmhm.github.io/" -D

生成新文件：
hugo new post/second.md
```
### 写blog操作步骤

1. 用hugo new 生成新文件test.md；
2. 编辑my_blog/content/post/test.md，添加各种属性内容，用单引号‘’；
3. 在本地服务器打开的状态下，是可以实时的看到网页的变化的，若满足要求，用hugo -t hyde-y --baseURL "https://****.github.io/" -D生成静态网页，然后在public目录下面再用git命令：add，commit，push 到 ****.github.io/仓库，现在既可以在玩个上看到新的内容了。
4. cd 到上一目录my_blog/, 在此也可以将源文件放到一个仓库。

