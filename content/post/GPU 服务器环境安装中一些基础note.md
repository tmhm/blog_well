+++ 
title = "GPU 服务器环境安装中一些基础note" 
date = "Mon, 21 Nov 2016 06:29:00 GMT" 
categories = ["GPU"] 
description = " GPU服务器配置记录。" 
+++ 


### GPU 服务器：

添加组，用户，并为之新建主目录。

```
c302@c302-dl:~$ sudo addgroup testgroup
Adding group `testgroup' (GID 1001) ...
Done.
c302@c302-dl:~$ sudo useradd testuser -g testgroup -m
```

新建密码

`passwd testuser
`

###  安装anaconda环境

官网下载之，[https://www.continuum.io/downloads](https://www.continuum.io/downloads)

安装，修改环境变量
`export PATH=/home/c302/anaconda2/bin:$PATH
`

###  Ubuntu Linux系统环境变量配置文件：
> /etc/profile : 在登录时,操作系统定制用户环境时使用的第一个文件 ,此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。

> /etc /environment : 在登录时操作系统使用的第二个文件, 系统在读取你自己的profile前,设置环境文件的环境变量。

> ~/.profile :  在登录时用到的第三个文件 是.profile文件,每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。

> /etc/bashrc : 为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取.

> ~/.bashrc : 该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。

修改~/.profile 文件，添加export PATH=/home/c302/anaconda2/bin:$PATH


###  安装pybrain 0.33

`
pip install pybrain
`

pip安装的是0.3版本的，我想要0.33的

卸之，运行：

`pip uninstall pybrain
`

用conda安装，

`conda install pybrain
`

> 提示：anaconda search -t conda pybrain

运行：

`anaconda search -t conda pybrain
`

> mq/pybrain                |    0.3.3 | conda           | linux-64, win-32, win-64, linux-32, osx-64

运行：

`conda install -c https://conda.anaconda.org/mq pybrain
`

###  挂载硬盘

```
mount  /dev/sdb1 /data1
mount  /dev/sdc1 /data2
mount  /dev/sdd1 /data3
```

上面是手动挂载

编辑 vi /etc/fstab 添加：

```
/dev/sdb1 /data1 xfs defaults 0 0
/dev/sdc1 /data2 xfs defaults 0 0
/dev/sdd1 /data3 xfs defaults 0 0
```

保存退出

这样重启后会自动挂载


###  安装 Nvidia GTX 1080 驱动 367.27

`sudo add-apt-repository ppa:graphics-drivers/ppa
`

出现警告，enter继续

```
sudo apt-get update
sudo apt-get install nvidia-367
sudo apt-get install mesa-common-dev
sudo apt-get install freeglut3-dev
```

之后重启系统让GTX1080显卡驱动生效。

运行：

`nvidia-smi
`

![](http://i.imgur.com/TUlwTFw.png)


重启进不了ubuntu 的桌面，输入密码又返回登陆界面，但是可以用ssh远程登陆。

###  新安装的系统，14.04。输入密码后又返回登录界面

1. sudo rm -r .Xauthority*  （Xauthority文件在/home/用户名/.Xauthority）

	**NOTE:** reboot之后, .Xauthorit 文件会再次自动生成。

2. 想起在/etc/profile 文件添加的export PATH=/home/c302/anaconda2/bin:$PATH
删除之， 再次reboot，没用，还是不能登录

3. 有博主说，将.Xauthority 文件修改拥有者变为用户。按下shift + ctrl + F7切换回图形登陆界面登陆即可。但是实测还是不行。

4. 安装gdm，sudo apt-get install gdm， 在安转的过程中会出现选择默认启动顺序，选择gdm。，启动直接黑屏了。小白无奈，再次卸载gdm。

5. 重新安装Unity，依次运行下面命令，尝试解决系统冻结的问题， 貌似对我还是没用

	```
    sudo apt-get update
    sudo apt-get install --reinstall ubuntu-desktop
    sudo apt-get install unity
    sudo shutdown -r now
```

6. 粗暴的方案，卸之，成功解决Nvidia显卡的Unity冻结问题

	```
    sudo apt-get update
    sudo apt-get install --reinstall ubuntu-desktop
    sudo apt-get install unity
    sudo apt-get remove --purge nvidia*
    sudo shutdown -r now
	```

删除了nvidia的驱动，重启之后Unity桌面成功恢复了。
nvidia-smi 已经无效。


###  teamviewer 安装

在网站下载teamviewer [https://www.teamviewer.com/zhcn/download/linux/](https://www.teamviewer.com/zhcn/download/linux/)

或者直接用wget下之，

`wget http://download.teamviewer.com/download/teamviewer_linux_x64.deb
`

然后执行

`sudo dpkg -i teamviewer_linux*.deb;
`

会有提示需要执行：`sudo apt-get -f install`

执行之，

`sudo apt-get -f install`


即可以通过teamviewer 远程访问图形界面

###  安装TORCS_gym等相关环境

请参考上一文章记录[用Keras 和 DDPG play TORCS（环境配置篇）](http://www.cnblogs.com/Qwells/p/6001699.html)

**NOTE:**

1. 注意权限问题，用sudo;
2. 添加两个float;
3. 安装keras 和tensorflow的时候，注意pip以及其他的一些版本依赖问题，一劳永逸的做法是将anaconda 组件升级到最新，即可。否则按照提示将pip升级，然后用sudo pip安装。



