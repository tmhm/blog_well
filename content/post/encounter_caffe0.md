+++
tags = ['caffe', 'machine learning']
categories = [machine-learning]
title = "Caffe初识笔记"
date = "2017-08-12T10:49:29+08:00"
description = "接触caffe源代码有一段时光了，周末，趁记忆仍新，将其部分内容做适量记录，仅当总结。"

+++

### 概述

- [Caffe](http://caffe.berkeleyvision.org/)是[Yangqing Jia](http://daggerfs.com/)在伯克利期间所作，目前属于比较流行的深度学习开源框架；
- 底层模块采用c++编码，执行效率较高，故在工业界的部署使用比较常见；
- 其源代码的模块化程度较高，对于初学者可以较好的理解深度学习的一些概念；
- 目前由[BVLC](http://bair.berkeley.edu/)维护，社区支持尽管没有TensorFlow的多，资料较为完善，初学者的一般问题都能在Internet找到解答；
- [github 源码](https://github.com/BVLC/caffe)README是一个很好的入门材料；源码中的docs目录下有一些较详细的指导文档。
- [Caffe Tutorial](http://caffe.berkeleyvision.org/tutorial/)多读易善。



### 依赖安装

Caffe 的安装,官方有很详细的[教程](http://caffe.berkeleyvision.org/installation.html)。

另外，应该注意的地方有：

- boost >= 1.55,
	- 在Ubuntu上用apt-get install比较方便；
	- 在CentOS7系统上，yum install boost，其**最新只有boost-1.5.3**，需要手动安装。 boost make 的时候注意需要sudo。
- cuda和cuDNN的版本注意匹配，详情查看NVIDIA 开发者官网；注意cuDNN不要装多个，请**确定只安装一份**即好。
- 注意安装Python 和 numpy（同样注意不要存在多个版本）， 推荐Anaconda2， 我一般安装Anaconda2在自己的home目录，不加路径到`PATH`,然后使用`~/anaconda2/bin/python`调用相关程序。BTW，推荐jupyter-notebook，可以配置远程访问服务器，可以比较方便调试一些算法的步骤。
- opencv2 可以在anaconda里面用`conda，pip install`，或者[anaconda::cloud](https://anaconda.org/anaconda/opencv/files)手动搜索下载，然后可以用`conda install  **.tar.bz2` 安装。
- 按照官方的Installation list 确保安装所有的依赖。

### 编译

一般来说按照官方的安装说明，安装好各种依赖，git clone [github Caffe源码](https://github.com/BVLC/caffe)。然后在 `Makefile.config` 配置好自己的相关设置，就可以执行编译。

```
make all -j99   
make pycaffe -j99
```
**注意make 是否采用sudo是有区别的，sudo 会调用管理员的各种设置，可能会出现找不到一些依赖包。**

config文件尽管写的非常清楚，初学者得非常注意，不然可能会出现明明安装了该包，确找不到相关的文件。本人就踩此坑多次。注意include头文件和lib的包含位置，`/usr/lib64`等。手动加上自己自定义安装的位置，BTW，CentOS安装locate，需要`sudo yum install mlocate && updatedb`.CebtOS下locate 改名为mlocate。

### 初次运行代码

如果前面的编译顺利，可以运行[MNIST例子](https://github.com/BVLC/caffe/tree/master/examples/mnist)体验下著名的[手写体识别](http://yann.lecun.com/exdb/mnist/)

```
cd $CAFFE_ROOT

./data/mnist/get_mnist.sh
./examples/mnist/create_mnist.sh

// 待生成mnist_train_lmdb, 和 mnist_test_lmdb 数据文件，即可开始训练。
./examples/mnist/train_lenet.sh
```

如果你下载了[caffe-ssd的源码](https://github.com/weiliu89/caffe/tree/ssd)。先按照README文件，create list && data，然后可以通过运行`python examples/ssd/ssd_pascal.py`来生成`train/test/deploy/solver.prototxt`文件进行模型的训练。以上四个文件是在利用Caffe平台已有的网络层做深度模型需要编辑的主要文件。

**Note：**假如此处的example运行失败，而前面的编译`make all && make pycaffe`均成功，很可能存在的一个问题是，动态链接库没配置好。
可以通过下面指令check：

```
cd build/tools
ldd caffe | grep not

// 此处会列出动态链接库没有找到的.lib.so文件
// 一般可以通过locate找到其位置，通过export加入到 LD_LIBRARY_PATH
// 也可以将下面export 放在个人home/.profile 文件中，再resource（简写.）一下
export LD_LIBRARY_PATH=/PATH/TO/YOUR/lib:$LD_LIBRARY_PATH 

```

### 源码结构

```
tree -L 2
（caffe-ssd示例，删除了部分内容）
├── Makefile.config // 配置文件，是否用cuDNN，设置include和lib的路径等
├── build -> .build_release
├── data // 保存训练data 的目录
│   ├── ILSVRC2016。。。
│   └── mnist
├── distribute // build生成的文件
│   ├── bin
│   └── lib
├── docs // 主要包括一些安装指导和入门教程文件
│   ├── install_osx.md // 安装帮助文件
│   └── tutorial。。。。 // 网页版 tutorial 的同源文件
├── examples //一些训练例子，推荐用jupyter看.ipynb文件
│   ├── 00-classification.ipynb
│   ├── ssd_detect.ipynb 。。。
│   └── web_demo
├── include // 主要包括一些源码生成的.hpp文件
│   └── caffe
├── matlab // matlab接口
├── models // model，即 train/test/deploy/solver.prototxt 等配置文件
│   ├── bvlc_alexnet
│   └── finetune_flickr_style
├── python // python接口
│   ├── caffe
│   ├── classify.py
│   ├── detect.py
│   ├── draw_net.py  // 将train.prototxt 文件画出模型的结构图片，很实用。
│   └── requirements.txt
├── scripts	// 一些有用的脚本
│   ├── create_annoset.py // 生成IMDB数据格式的训练数据用到。
│   └── upload_model_to_gist.sh
├── src //源码的主要位置，src/caffe/proto/caffe.proto中定义caffe用到的各种参量。
│   ├── caffe // 其layers目录下，有各种网络层的前后向计算，分cpu和gpu版。
│   └── gtest
└── tools
    ├── caffe.cpp
    ├── convert_annoset.cpp  // create data 用到
    ├── convert_imageset.cpp
    ├── create_label_map.cpp
    ├── device_query.cpp   //查询GPU信息
    ├── extract_features.cpp
    └── finetune_net.cpp
```












