+++ 
title = "ubuntu12.04LTS 系统下QT 4.8.6和QT creator 2.5.2的安装" 
date = "Wed, 06 Jan 2016 10:30:00 GMT" 
tags = ["ubuntu"] 
categories = ["OS"]
description = "ubuntu12.04LTS安装以及卸载 QT4.8.6和QT creator2.5.2。" 
+++ 

鉴于，下载QT5.5安装，编译总是有问题，可能是配置不正确。
于是按照论坛的一些资料，就换回QT4版本，具体实施步骤如下：
在qt官网`http://download.qt.io/archive/`下的
qt4.8.6：`http://download.qt.io/archive/qt/4.8/4.8.6/qt-everywhere-opensource-src-4.8.6.tar.gz`
和 qt creator2.5.2：` http://download.qt.io/archive/qtcreator/2.5/qt-creator-linux-x86-opensource-2.5.2.bin`


- 在终端依次输入以下命令：

```
tar xzvf qt-everywhere-opensource-src-4.8.6.tar.gz
cd qt-everywhere-opensource-src-4.8.6
sudo apt-get install g++
sudo apt-get install libX11-dev libXext-dev libXtst-dev
配置QT Library ，输入命令： ./configure
```
<p>配置完成，会得到如下终端界面：</p>

```
Qt is now configured for building. Just run 'make'.
Once everything is built, you must run 'make install'.
Qt will be installed into /usr/local/Trolltech/Qt-4.8.6
 
To reconfigure, run 'make confclean' and 'configure'.
```

- 然后再输入：`make `
生成QT库，以及编译所有演示程序

<p>这里的时间挺长的。。。。。</p>

- 执行安装命令：

```
sudo make install  
（卸载的时候也是在此目录下，执行：sudo make uninstall）
```

- QT默认安装在/usr/local/Trolltech/Qt-4.8.6目录里面，进入该目录，测试

```
cd /usr/local/Trolltech/Qt-4.8.6
cd bin
./qmake -v
```


<p>输出：</p>

```
tl@tl-virtual-machine:/usr/local/Trolltech/Qt-4.8.6/bin$ ./qmake -v
QMake version 2.01a
Using Qt version 4.8.6 in /usr/local/Trolltech/Qt-4.8.6/lib
```

<p>如果出现正确版本信息，表示安装成功。</p>

<p>我的ubuntu安装了多个qmake，测试的结果是这样子的。（注意执行qmake -v命令的路径，因为4.8.6并没有设置环境变量，故得在qmake程序所在目录路径执行。）</p>
<div class="cnblogs_code">
<pre>[linux-devkit]:/media/tl437x/qtcreator-<span style="color: #800080;">2.6</span>.<span style="color: #800080;">1</span>/bin&gt; qmake -<span style="color: #000000;">v
QMake version </span><span style="color: #800080;">3.0</span><span style="color: #000000;">
Using Qt version </span><span style="color: #800080;">5.4</span>.<span style="color: #800080;">1</span> <span style="color: #0000ff;">in</span> /media/tl437x/ti-processor-sdk-linux-am437x-evm-<span style="color: #800080;">01.00</span>.<span style="color: #800080;">00.03</span>/linux-devkit/sysroots/cortexa9t2hf-vfp-neon-linux-gnueabi/usr/lib</pre>
</div>
<div class="cnblogs_code">
<pre>tl@tl-<span style="color: #0000ff;">virtual</span>-machine:~$ qmake -<span style="color: #000000;">v
QMake version </span><span style="color: #800080;">2</span><span style="color: #000000;">.01a
Using Qt version </span><span style="color: #800080;">4.8</span>.<span style="color: #800080;">1</span> <span style="color: #0000ff;">in</span> /usr/lib/i386-linux-gnu</pre>

<div class="cnblogs_code">
<pre>tl@tl-<span style="color: #0000ff;">virtual</span>-machine:/usr/local/Trolltech/Qt-<span style="color: #800080;">4.8</span>.<span style="color: #800080;">6</span>/bin$ ./qmake -<span style="color: #000000;">v
QMake version </span><span style="color: #800080;">2</span><span style="color: #000000;">.01a
Using Qt version </span><span style="color: #800080;">4.8</span>.<span style="color: #800080;">6</span> <span style="color: #0000ff;">in</span> /usr/local/Trolltech/Qt-<span style="color: #800080;">4.8</span>.<span style="color: #

<p>添加环境变量，（需要在任意目录下使用，才添加环境变量）</p>
<p>首先需要设置用户环境变量，</p>


<p>弹出一个编辑窗口，在末尾添加以下代码，</p>
<div class="cnblogs_code">
<pre>export QTDIR=/usr/local/Trolltech/Qt-<span style="color: #800080;">4.8</span>.<span style="color: #800080;">6</span><span style="color: #000000;">
export PATH</span>=$QTDIR/<span style="color: #000000;">bin:$PATH
export MANPATH</span>=$QTDIR/<span style="color: #000000;">man:$MANPATH
export LD_LIBRARY_PATH</span>=$QTDIR/lib:$LD_LIBRARY_PATH  </pre>
</div>
<p>然后设置root用户的环境变量，设置root用户的环境变量需要root权限，所以要加sudo，</p>


<div class="cnblogs_code">
<pre>sudo gedit /etc/profile  </pre>
</div>
<p>添加：</p>
<div class="cnblogs_code">
<pre>export QTDIR=/usr/local/Trolltech/Qt-<span style="color: #800080;">4.8</span>.<span style="color: #800080;">6</span><span style="color: #000000;">
export PATH</span>=$QTDIR/<span style="color: #000000;">bin:$PATH
export MANPATH</span>=$QTDIR/<span style="color: #000000;">man:$MANPATH
export LD_LIBRARY_PATH</span>=$QTDIR/lib:$LD_LIBRARY_PATH </pre>
</div>
<p>保存后退出，然后重启电脑，在终端的任意目录下输入qmake， 如果能出现正确信息，表示环境变量配置成功。</p>
<p><span style="color: #ff0000;">&nbsp;此处，我并没有添加系统变量！，下面直接通过qtcreator工具选项添加。</span></p>

<li><strong>安装qtcreator</strong></li>

<p>安装路径：/home/tl/qtcreator-2.5.2</p>
<div class="cnblogs_code">
<pre>chmod a+x qt-creator-linux-x86-opensource-<span style="color: #800080;">2.5</span>.<span style="color: #800080;">2</span><span style="color: #000000;">.bin<br />
 .</span>/qt-creator-linux-x86-opensource-<span style="color: #800080;">2.5</span>.<span style="color: #800080;">2</span>.bin  </pre>
</div>
<p><em>&nbsp;卸载的时候在安装目录下/home/tl/qtcreator-2.5.2/bin，执行:uninstall</em></p>


<p>打开qtcreator，添加qt4.8.6，即刚刚安装好的那个版本。</p>


<p><img src="http://images2015.cnblogs.com/blog/781469/201601/781469-20160106172901965-4851180.jpg" alt="" width="802" height="498" /></p>
<p><img style="vertical-align: middle;" src="http://images2015.cnblogs.com/blog/781469/201601/781469-20160106182922887-471010331.jpg" alt="" width="803" height="515" /></p>
<p>成功运行，第一个桌面程序。</p>





