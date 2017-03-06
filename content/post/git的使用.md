+++ 
title = "git的使用" 
date = "Mon, 08 Aug 2016 09:13:00 GMT" 
tags = ["git"] 
categories = ["Tools"]
description = " git 入门" 
+++ 



##### 1.创建SSH Key

`ssh-keygen -t rsa -C "5733*****@qq.com"
`

在主用户目录下生成 *.ssh* 文件夹，该目录下有 *id\_rsa* 和 *id\_rsa.pub* 两个文件。

*id\_rsa* 是私钥， *id\_rsa.pub* 是公钥

##### 2.Add SSH Key

登陆GitHub，打开 Account settings --> SSH and GPG Keys页面：Add SSH Key, 填上Title， 在Key文本框粘贴*id\_rsa.pub* 的内容。

##### 3.在远程服务器初始化仓库 swrd

之后它会给出如下提示：

**or create a new repository on the command line**

```
echo "# swrd" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:tmhm/swrd.git
git push -u origin master

```
**or push an existing repository from the command line**

```
git remote add origin git@github.com:tmhm/swrd.git
git push -u origin master
```

##### 4.在本地 git init并连接远程
```
git init
git add --all
git commit -m "init commit"
git remote add origin git@github.com:tmhm/swrd.git
```

##### 5.push 到远程

```
git push -u origin master
```

#####  .gitingore文件

- 不想把所有的文件都track到远程库，可以通过添加.gitignore文件忽略（全局有效，可以忽略文件本身）
- 或者通过vi ./.git/info/exclude（只在本地库有效，他不会影响到其他人。也不会提交到版本库中去 ）
- 参考github的轮子： [https://github.com/github/gitignore](https://github.com/github/gitignore)



