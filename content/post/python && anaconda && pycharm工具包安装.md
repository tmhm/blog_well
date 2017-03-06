+++ 
title = "python && anaconda && pycharm工具包安装" 
date = "Sun, 24 Apr 2016 13:24:00 GMT" 
tags = ["Python"] 
categories = ["Language"]
description = "anaconda环境下，python包的安装记录" 
+++ 

`$ conda update conda
`

####  更新pip
`python -m pip install --upgrade pip
`

####  更新所有

`conda update --all
`

####  安装ffmpeg

`conda install ffmpeg
`

不存在该包，用`anaconda search`

`
[Anaconda2] C:\Users\xwei>anaconda search -t conda ffmpeg
`

`groakat/ffmpeg-dev        |    2.4.3 | conda           | linux-64, win-64,osx-64
`

`[Anaconda2] C:\Users\xwei>conda install -c https://conda.anaconda.org/groakat ffmpeg-dev`


####  安装theano
[source for win-install](http://deeplearning.net/software/theano/install_windows.html#install-windows)

[source for install](http://deeplearning.net/software/theano/install.html)

[stack overflow](http://stackoverflow.com/questions/33671453/g-not-detected-while-data-set-goes-larger-is-there-any-limit-to-matrix-size)

[Anaconda2] C:\Users\xwei\Desktop\lstm-test>
>anaconda search -t conda theano


`jaikumarm/theano          |    0.8.2 | conda           | linux-64, win-32, win-64, linux-32, osx-64  : Optimizing compiler for evaluating mathematical expressions on CPUs and GPUs.`


[Anaconda2] C:\Users\xwei\Desktop\lstm-test>
>conda install -c https://conda.anaconda.org/jaikumarm theano

[Anaconda2] C:\Users\xwei\Desktop\lstm-test>python lstm.py


>WARNING (theano.configdefaults): g++ not detected ! Theano will be unable to execute optimized C-implementations (for both CPU and GPU) and will default to Python implementations. Performance will be severely degraded. To remove this warning, set Theano flags cxx to an empty string.



* [Anaconda2] C:\Users\xwei> **conda install mingw libpython**
* 之后即可以在anaconda Prompt 里面直接调用theano ，包括g++
* 运行[LSTM Networks for Sentiment Analysis](http://www.deeplearning.net/tutorial/lstm.html#lstm) 明显快了很多很多，直接在cpu下运行纯python代码，一两分钟才出来第一次迭代， 安装之后，运行c代码的时候，基本上两三秒一次迭代。

####  安装出现Anaconda Python installation error
![](http://images2015.cnblogs.com/blog/781469/201606/781469-20160612222301152-884209750.png)

解决参考[stackoverflow](http://stackoverflow.com/questions/34780267/anaconda-python-installation-error)

First, open a DOS prompt and admin rights. Then, go to your Anaconda2\Scripts folder.

> conda update conda

and allow all updates. One of the updates should be menuinst.

Then, change to the Anaconda2\Lib directory, and type in the following command:

> ..\python _nsis.py mkmenus

Wait for this to complete, then check your Start menu for the new shortcuts.



