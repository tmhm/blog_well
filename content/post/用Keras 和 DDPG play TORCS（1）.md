+++ 
title = "用Keras 和 DDPG play TORCS（1）" 
date = "Wed, 26 Oct 2016 12:06:00 GMT" 
categories = ["TORCS"] 
description = "DDPG算法的学习记录之配置篇" 
+++ 

原作者[Using Keras and Deep Deterministic Policy Gradient to play TORCS](https://yanpanlau.github.io/2016/10/11/Torcs-Keras.html)


配置gym-torcs，[参考](http://www.jianshu.com/p/a3432c0e1ef2)

> 由于使用的环境是ubuntu 14.04 desktop版，故不需要安装opencv。
>
####  安装一些依赖包：

```
sudo apt-get install xautomation
sudo pip install numpy
sudo pip install gym
```

####  下载[gym_torcs源码](https://github.com/ugo-nama-kun/gym_torcs)

- 然后将

```
gym_torcs/vtorcs-RL-color/src/modules/simu/simuv2/simu.cpp
中第64行替换为
if (isnan((float)(car->ctrl->gear)) || isinf(((float)(car->ctrl->gear)))) car->ctrl->gear = 0;
```
即添加两个（float），否则，下一步make的时候会出现error，安转失败

- cd 到 gym_torcs/vtorcs-Rl-color目录，

执行以下命令：

```
sudo apt-get install libglib2.0-dev  libgl1-mesa-dev libglu1-mesa-dev  freeglut3-dev  libplib-dev  libopenal-dev libalut-dev libxi-dev libxmu-dev libxrender-dev  libxrandr-dev libpng12-dev

./configure

make

sudo make install

sudo make datainstall
```
输入命令
        `torcs`

即将打开，熟悉的TORCS 界面， 已打patch scr


![](http://images2015.cnblogs.com/blog/781469/201611/781469-20161121150329987-1053577076.bmp)


###  [DDPG源码](https://github.com/yanpanlau/DDPG-Keras-Torcs)

```
pip install keras
pip install tensorflow

git clone https://github.com/yanpanlau/DDPG-Keras-Torcs.git
cd DDPG-Keras-Torcs
cp *.* ../gym_torcs
cd ../gym_torcs
```

下面运行ddpg.py

`python ddpg.py`

开始看到漂亮的训练过程啦


![](http://images2015.cnblogs.com/blog/781469/201611/781469-20161121150308612-1273258933.bmp)


*在笔记本上运行ddpg.py的时候会出现 keras.backecd中没有set_session模块，初步猜想应该是GPU的问题，在带GPU台式机的虚拟机ubuntu14.04上，即可以正常运行。*

- 注意到，笔记本中一个细节是：Using Theano backend
- 而，虚拟机中显示的是：Using tensorflow backend
- 在[keras/backend主页](https://keras.io/backend/)找到问题所在,是keras的配置问题
- 打开~/.keras/keras.json，把backend选项，修改Theano为tensorflow，然后wq，退出即可。

```
{
    "image_dim_ordering": "tf",
    "epsilon": 1e-07,
    "floatx": "float32",
    "backend": "tensorflow"
}
```


####  修改默认python

- 删除系统自带的python软链接

	`rm /user/bin/python`

- 建立新安装的python 的软链接

	`ln -s ~/anaconda2/bin/python /user/bin/python`

现在打开命令行python 即是安装在~/anaconda2/bin/python 的python程序



