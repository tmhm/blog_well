+++ 
title = "DL服务器主机环境配置 ubuntu14.04 &&  GTX1080 && Cuda8.0 解决桌面重复登录" 
date = "Mon, 21 Nov 2016 12:15:00 GMT" 
tags = ["DL"] 
categories = ["Machine Learning"]
description = "前面部分是自己的记录，后面的方案部分是成功安装GPU驱动+解决桌面重复登录等" 
+++ 


*前面部分是自己的记录，后面方案部分是成功安装驱动+桌面的正解*

问题的开始在于：**登录不了桌面，停留在重复输入密码界面**


[博文中](http://blog.csdn.net/nothinglefttosay/article/details/45095125)分析的**结论：**
**虚拟机中不能直接调用物理显卡进行 CUDA 编程；虚拟机中运行 CUDA 需要硬件和软件的配合才能使用，对于一般使用者可能暂时不太可能的。**


参考博文：

[深度学习主机环境配置: Ubuntu16.04+Nvidia GTX 1080+CUDA8.0](http://www.52nlp.cn/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E4%B8%BB%E6%9C%BA%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-ubuntu-16-04-nvidia-gtx-1080-cuda-8)

[深度学习主机环境配置: Ubuntu16.04+GeForce GTX 1080+TensorFlow](http://www.52nlp.cn/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E4%B8%BB%E6%9C%BA%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-ubuntu16-04-geforce-gtx1080-tensorflow)

[ubuntu14.04+cuda8.0（GTX1080）+caffe安装](http://www.codexiu.cn/linux/blog/27993/)

[如何搭建一台深度学习服务器](https://www.r-bloggers.com/lang/chinese/2042)


Ctrl+alt+F1进入字符界面，关闭图形界面

```
sudo service lightdm stop //必须有，不然会安装失败
sudo /etc/init.d/lightdm stop //一样的命令

sudo chmod 755 NVIDIA-Linux-x86_64-367.27.run  //获取权限
sudo ./NVIDIA-Linux-x86_64-367.27.run  //安装驱动
```

Accept
Continue installation
安装完成之后

`sudo service lightdm start`

图形界面出现，然后关机，由让人重复输入密码，登录不了

[博主](http://nemesisdesign.net/blog/coding/how-reinstall-nvidia-drivers-linux-ubuntu/)说

```
$ sudo /etc/init.d/gdm stop
$ sudo nvidia-installer --update
$ sudo /etc/init.d/gdm start
```
升级到375版本， 还是没用，启动进入不了桌面，重复登录


###  [Install driver 367](https://kusemanohar.wordpress.com/2016/07/29/gtx-1080-on-ubuntu-14-04-trusty/)

Uninstall previous nvidia drivers.

`$ sudo apt-get purge nvidia-*
`

Stop light gdm (graphical interface)

`$ sudo service lightgdm
`

Go to tty (CTRL+ALT+F1). Set your init state to 3 (text only mode). It is important to do this. Note these commands on a paper or something. I experienced sometimes the tty does not show with the newest driver. I just ssh to my PC as a way around.

`$ sudo init 3
`

Log in to tty and cd to the directory where your have downloaded the driver.

`$ sudo ./NVIDIA-Linux-x86_64-367.35.run
`

It will ask if you want to install 32-bit libraries, say no (assuming you do not have a 32-bit OS, hopefully. If you do have a 32-bit OS it is a good idea to upgrade…)


In a few minutes it is done….smooth. Reboot your PC
`$ sudo reboot
`

update 之后还是不能进 图形界面

Uninstall previous nvidia drivers.

```
sudo apt-get purge nvidia-*
sudo apt-get autoremove
sudo apt-get --purge remove nvidia-*
```

remove 之后，

`nvidia-smi
`

还是能看到gpu的。why？

卸载不了？

```
sudo apt-get install nvidia-prime

$ sudo /etc/init.d/lightdm stop
$ sudo nvidia-installer --update
$ sudo /etc/init.d/lightdm start
```

**升级到375版本， 还是没用，启动进入不了桌面，重复登录**


> 有人说，安装必须要在安装桌面前安装GTX 1080 driver，后面方案验证来看， 那个参数才是关键。


###  解决方法

利用sudo gedit /etc/modprobe.d/blacklist-nouveau.conf新建blacklist-nouveau.conf文件，输入命令

```
blacklist nouveau

blacklist lbm-nouveau

options nouveau modeset=0

alias nouveau off

alias lbm-nouveau off
```

保存并退出。这一步是为了禁掉Ubuntu自带开源驱动nouveau。之后sudo reboot重启系统。在终端执行命令
`lsmod | grep nouveau
`

查看nouveau模块是否被加载。如果什么都没输出，则执行下一步。


根本问题在于 参数：  **--no-opengl-files**

```
sudo /etc/init.d/lightdm stop
sudo ./NVIDIA-Linux-x86_64-375.20.run --no-opengl-files
sudo /etc/init.d/lightdm start
```


即可以正常登录界面了！！

在安装过程中的选项：

Accept

Continue installation

> register the kernel moudle sources with DKMS?

NO

 > Would you like to run the nvidia-xconfig utility to automatically update your X Configuration file so set the NVIDIA X driver will be used when you restart X?

NO

> Install 32-Bit compatibility [libraries?参考](https://elementaryforums.com/index.php?threads/howto-install-latest-nvidia-driver-on-linux-without-getting-black-screen.7/)

NO
![](http://i.imgur.com/5DHQbMl.png)

###  cuda8.0安装

运行

`sudo sh cuda_8.0.44_linux.run
`

选项如下所示：

```
Description

This package includes over 100+ CUDA examples that demonstrate
various CUDA programming principles, and efficient CUDA
implementation of algorithms in specific application domains.
The NVIDIA CUDA Samples License Agreement is available in
Do you accept the previously read EULA?
accept/decline/quit: accept

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 367.48?
(y)es/(n)o/(q)uit: n

Install the CUDA 8.0 Toolkit?
(y)es/(n)o/(q)uit: y

Enter Toolkit Location
 [ default is /usr/local/cuda-8.0 ]:

Do you want to install a symbolic link at /usr/local/cuda?
(y)es/(n)o/(q)uit: y

Install the CUDA 8.0 Samples?
(y)es/(n)o/(q)uit: y

Enter CUDA Samples Location
 [ default is /home/c302 ]:

Installing the CUDA Toolkit in /usr/local/cuda-8.0 ...
Installing the CUDA Samples in /home/c302 ...
Copying samples to /home/c302/NVIDIA_CUDA-8.0_Samples now...
Finished copying samples.

===========
= Summary =
===========

Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-8.0
Samples:  Installed in /home/c302

Please make sure that
 -   PATH includes /usr/local/cuda-8.0/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-8.0/lib64, or, add /usr/local/cuda-8.0/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run the uninstall script in /usr/local/cuda-8.0/bin

Please see CUDA_Installation_Guide_Linux.pdf in /usr/local/cuda-8.0/doc/pdf for detailed information on setting up CUDA.

***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least 361.00 is required for CUDA 8.0 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run -silent -driver

Logfile is /tmp/cuda_install_9045.log
    
```

#### 设置环境变量

```
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH
```

添加系统变量修改到系统文件
`sudo vi /etc/profile
`

在最后添加上面两句，然后保存。使立即生效
`sudo ldconfig //环境变量立即生效
`

####  验证 cuda

```
c302@c302-dl:~/Downloads$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2016 NVIDIA Corporation
Built on Sun_Sep__4_22:14:01_CDT_2016
Cuda compilation tools, release 8.0, V8.0.44
```

###  测试cuda的samples

```
cd ‘/home/zhou/NVIDIA_CUDA-8.0_Samples’
make  //这里需要点时间
```

最后显示：
> make[1]: Leaving directory `/home/c302/NVIDIA_CUDA-8.0_Samples/7_CUDALibraries/MersenneTwisterGP11213'

>Finished building CUDA samples

`
        cd 0_Simple/matrixMul
`

运行测试如下：

```
c302@c302-dl:~/NVIDIA_CUDA-8.0_Samples/0_Simple/matrixMul$ ./matrixMul
[Matrix Multiply Using CUDA] - Starting...
GPU Device 0: "GeForce GTX 1080" with compute capability 6.1

MatrixA(320,320), MatrixB(640,320)
Computing result using CUDA Kernel...
done
Performance= 1109.06 GFlop/s, Time= 0.118 msec, Size= 131072000 Ops, WorkgroupSize= 1024 threads/block
Checking computed result for correctness: Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.

```

![](http://i.imgur.com/iRwuetR.png)


下一篇将会是安装cuDNN、tensorflow等lib



