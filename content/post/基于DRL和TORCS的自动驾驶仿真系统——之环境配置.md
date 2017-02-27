+++ 
title = "基于DRL和TORCS的自动驾驶仿真系统——之环境配置" 
date = "Wed, 08 Feb 2017 09:13:00 GMT" 
categories = ["自动驾驶"] 
description = "玩TORCS和DRL差不多有一整年了，开始的摸爬滚打都是不断碰壁过来的，近来在参与[CMU的DRL10703课程](https://katefvision.github.io/)学习和翻译志愿者工作，也将自己以前的一些工作做一些备忘，以作为有兴趣同学的参考。" 
+++ 

*玩TORCS和DRL差不多有一整年了，开始的摸爬滚打都是不断碰壁过来的，近来在参与[CMU的DRL10703课程](https://katefvision.github.io/)学习和翻译志愿者工作，也将自己以前的一些工作做一些备忘，以作为有兴趣同学的参考。*

### TORCS仿真器平台安装
仿真器平台主要包括两步：安装TORCS，然后添加锦标赛用的patch。

#### TORCS仿真器的安装

The open racing car simulator（TORCS）[主页](http://torcs.sourceforge.net/)

仿真器源文件[下载地址](https://sourceforge.net/projects/torcs/files/all-in-one/)

支持windows和linux，windows下面的安装有集成的安装源文件包；linux系统下则需要自己编译安装一些依赖库，否则在下一步打patch的时候不成功，原因是linux的包是编译后的文件，不包括源文件，windows无此问题。

示例是在win-64bits系统下安装torcs-1.3.4。

跟普通应用安装类似，安装后，可在桌面创建快捷方式 。
打开安装好的TORCS，如下图所示。
![](http://i.imgur.com/kJvZTvr.png)

某些简单问题，在这里可能可以找到答案：
[http://torcs.sourceforge.net/index.php?name=Sections&op=viewarticle&artid=30](http://torcs.sourceforge.net/index.php?name=Sections&op=viewarticle&artid=30)

简单配置How-to教程可参考（开始使用默认配置即可）：
[http://torcs.sourceforge.net/index.php?name=Sections&op=listarticles&secid=4](http://torcs.sourceforge.net/index.php?name=Sections&op=listarticles&secid=4)

如何在仿真器上一步步实现一个简单ROBOT，教程可见：
[http://www.berniw.org/tutorials/robot/](http://www.berniw.org/tutorials/robot/)

#### Championship Platform的安装
为扩展仿真器平台用于我们的智能驾驶AI程序的开发，我们采用的是锦标赛平台的接口。在安装torcs之后，可以直接安装patch。实现安装源文件下载地址：
[https://sourceforge.net/projects/cig/files/SCR%20Championship/](https://sourceforge.net/projects/cig/files/SCR%20Championship/)

示例安装的是windows server patch2.0. 文件链接：
[https://sourceforge.net/projects/cig/files/SCR%20Championship/Server%20Windows/2.0/](https://sourceforge.net/projects/cig/files/SCR%20Championship/Server%20Windows/2.0/)

解压下载的patch.zip覆盖上一步安装torcs的安装文件，即可搭建一个服务器端。
 ![](http://i.imgur.com/Cx5dk9y.png)
上图中的wtorcs.exe即平台的入口地址。可将其快捷方式发送到桌面。打开该程序，现在可以配置我们的drivers。


路径是： Race --> Quick Race/ Practice --> Configure Race --> Select Track (Accept)--> Select Drivers (如1，用上下方向键选择scr…，然后点击select(选择和删除是同一按钮)，最后Accept即可)，如下图所示。
然后accept， New Race 即可以进入仿真器。
 ![](http://i.imgur.com/JQwQQXD.png)



我们的程序即可以通过scr车手来模拟控制，一些有用的配置可以参考。
Manual的官方文件: [http://arxiv.org/abs/1304.1672](http://arxiv.org/abs/1304.1672)

若有墙，可参考：
[https://www.yumpu.com/en/document/view/48269886/simulated-car-racing-championship-competition-software-manual](https://www.yumpu.com/en/document/view/48269886/simulated-car-racing-championship-competition-software-manual)

至此，TORCS仿真器平台安装完毕，下面搭建配套的软件IDE环境。



###  软件IDE环境搭建

#### Anaconda 的安装
Anaconda 是一个开源的，基于python的跨平台（windows，osx，liunx）科学计算平台，支持python2和python3，示例用的是基于python2的anaconda2-4.0.0

安装后，会创建一个Anaconda Prompt（一个类似DOS的命令行窗口），它可以像linux一样来通过命令管理各种科学计算包，执行命令等。
比如可以用conda ，pip等工具管理包

#### PyBrain的安装
Pybrain是一个基于python的机器学习模块，对强化学习的支持比较好，2015年底的时候，还只有很少的模块是专门做强化学习的，Pybrain就是定位在强化学习和神经网络，由于是个人的项目，维护更新比较慢，目前使用的是0.33版本。

在已安装的系统上运行**conda install pybrain**会显示是否已安装。

由于使用的台式机已经安装pybrain 0.33，故用笔记本示意安装过程，如下图所示.
![](http://i.imgur.com/D651H7i.png)

![](http://i.imgur.com/MHxTMPH.png)

![](http://i.imgur.com/eZVuF0d.png)

主要命令包括（字母均为小些形式）：

        1.      conda install pybrain
        2.      anaconda search -t conda pybrain
        3.      conda install -c https://conda.anaconda.org/mq pybrain
*Note： mq 是指包的发布者*


####  PyCharm 的安装
尽管Anaconda包含了一个Spyder 的IDE，个人感觉不太友好，故还是额外安装[PyCharm](https://www.jetbrains.com/pycharm/download/#section=windows)。PyCharm是JetBrains公司推出的一套基于python的跨平台工具。包含免费的社区版和收费的专业版，示例的是专业版，由于近期修改系统时间的漏洞已经修护（貌似最多就一年有效），建议直接安装社区版即可。与TORCS的接口程序在下一篇代码部分给出。



