+++
tags = []
categories = ["Tools"]
date = "2017-03-03T12:05:02+08:00"
description = "win10 升级后，主机在锁屏状态下，Teamviewer无法远程登录账户，防火墙已关闭，无人值守访问不了"
title = "Teamviewer无法远程登录主机用户"

+++

### 问题描述
win10 升级后，主机在锁屏状态下，Teamviewer无法远程登录账户，防火墙已关闭，无人值守访问不了

### 解决方式：

- 注意主从机的版本一致
- 注意开启了“授权轻松访问”，用金山毒霸查杀系统之后，会修复使Teamviewer 不具有管理员权限，导致开启轻松访问，并不能远程登录，出现的情况就是可以连接，就是登录不了，输入键盘没反应。
- 重装，装的时候开启无人值守，轻松访问，即帮你自动设置好了。如下示图。

![](/images/无人值守访问.jpg  "示图1")
![](/images/授权轻松访问.jpg "示图2") 

### 问题并未终结
- 当主机重启之后，Teamviewer的授权轻松访问的钩被去掉了，所以远程不能登录主机，即当主机锁屏的时候，Teamviewer可以连接到主机，但是远程不能登录主机账号。
- 重点应该在于开启**授权轻松访问**，而跟windows登录的关系不在于此处。如图

![](/images/teamviewer配置原因.jpg "示图3")

### 尝试方案

- 不能直接勾选【授权轻松访问】选项，显示需要管理员登录，于是设置程序的【兼容性】属性，“以管理员身份运行程序”。
- 并且勾选了随计算机开机启动，总是不能自动启动，可能是win10的安全配置所致，于是手动加入启动项：
	1. Win+R打开运行程序，运行`shell:startup`，
	2. 将teamviewer的快捷方式，拉入该文件目录。
	3. 手动加入启动设置完毕。然而，重启，并不能启动teamviewer12.并且**设置里面的授权轻松访问**已被清零！,现在**锁屏状态下再次不能访问登录!**


- 尝试**再次重装**后，在首页，将随windows 一同启动Teamviewer，分配设备，授权轻松访问全部选上，既可以在设置选项里面看到打开了授权访问。并且**重装后的teamviewer是可以锁屏登录的！**，可恶的win10设置！

### 待解决

 ubuntu 14.04下的Teamviewer也登录不了，本来用着好的Teamviewer，现在却登录不了。苦于不在主机旁，只能用命令重新安装之，参考了[博文](http://blog.csdn.net/dreamhai/article/details/57080531)，先在虚拟机上实现，却发现问题。

```
wget https://downloadus1.teamviewer.com/download/version_12x/teamviewer_12.0.71510_i386.deb

sudo dpkg -i teamviewer_12.0.71510_i386.deb
```

由于64bits系统和32bits程序的不兼容性，故需要添加32bits 架构
```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get -f install
```
在虚拟机ubuntu14.04上安装之。`sudo dpkg -i teamviewer_12.0.71510_i386.deb
`
总是显示安装错误！

```
(Reading database ... 324209 files and directories currently installed.)
Preparing to unpack teamviewer_12.0.71510_i386.deb ...
Unpacking teamviewer (12.0.71510) over (12.0.71510) ...
dpkg: dependency problems prevent configuration of teamviewer:
 teamviewer depends on libdbus-1-3.
 teamviewer depends on libsm6.

dpkg: error processing package teamviewer (--install):
 dependency problems - leaving unconfigured
Processing triggers for mime-support (3.54ubuntu1.1) ...
Processing triggers for gnome-menus (3.10.1-0ubuntu2) ...
Processing triggers for desktop-file-utils (0.22-1ubuntu1) ...
Processing triggers for bamfdaemon (0.5.1+14.04.20140409-0ubuntu1) ...
Rebuilding /usr/share/applications/bamf-2.index...
Processing triggers for hicolor-icon-theme (0.13-1) ...
Errors were encountered while processing:
 teamviewer
```

弹出没有启动daemon服务，启动之，
`sudo teamviewer --daemon start`， no work.

更加奇怪的是，服务器ubuntu 上的Teamviewer不登录任何账号的时候，可以直接远程访问了。

### 一些有用的命令

- 获取Teamviewer的ID`teamviewer --info`
- 设置密码`sudo teamviewer --passwd [newpw]`

**修改配置文件以接受licence**

1. 停止服务`sudo teamviewer --daemon stop`
2. 在`/opt/teamviewer/config/global.conf`文件最后添加两行
```
[int32] EulaAccepted = 1 
[int32] EulaAcceptedRevision = 6 
```
3. 重启服务`sudo teamviewer --daemon start`

### 粗暴解决

在一台新的win10系统下，发现teamviewer，可以重启使用，突然想起，曾经用驱动精灵和金山毒霸进行过优化，于是，卸载之，并取消他们的优化，再安装teamviewer，发现可以正常使用了。
**都是过度优化惹得祸！**