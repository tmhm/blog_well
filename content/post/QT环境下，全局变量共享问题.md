+++ 
title = "QT环境下，全局变量共享问题" 
date = "Tue, 08 Mar 2016 12:47:00 GMT" 
tags = ["技术"] 
categories = ["Tools"]
description = "QT环境下的全局变量共享" 
+++ 

<p><strong>开始的技术路线是：</strong></p>
<p>&nbsp;首先有两个主线程：</p>
<p>　　1，gui线程</p>
<p>　　2，等待客户端socket连接用的，listen线程</p>
<p>　　（当有客户端连接时，即creat一个新的线程2用来跟客户端通信，再来新的客户端，继续creat新的work线程3用于通信，以此类推，目前最多可以creat5个线程，即可以同时跟5台客户端通信，设置了监听的socket服务器最多监听5个队列。线程2继续listen）</p>

<p><strong>出现的问题是：</strong></p>
<p>　　用来通信的work线程3，收到一个来自客户端的xml文件，然后解析文件，并将其数据放在一个全局的静态struct数组里。数据正常，线程2和线程3均可以看到已经更新的数据值。但是，</p>
<p>　　当gui线程去访问时，发现数据并没有更新到其线程，在gui线程下改变该全局变量的数值，也并没有更新到

<p><em>一博主，有如此解释</em>：</p>
<div id="article_content" class="article_content"><a href="http://blog.csdn.net/lmh12506/article/details/8452700" target="_blank">其实在Linux 中，新建的线程并不是在原先的进程中，而是系统通过一个系统调用clone() 。该系统copy 了一个和原先进程完全一样的进程，并在这个进程中执行线程函数。不过这个copy 过程和fork 不一样。copy 后的进程和原先的进程共享了所有的变量，运行环境（clone的实现是可以指定新进程与老进程之间的共享关系，100%共享就表示创建了一个线程）。这样，原先进程中的变量变动在copy 后的进程中便能体现出来。</a></div>
<div class="bdsharebuttonbox tracking-ad bdshare-button-style0-16" data-mod="popu_172" data-bd-bind="1457096202549">&nbsp;</div>
<div id="digg">&nbsp;不过，问题并没有清晰化。</div>


<div>想到的解决方案暂时有两种：</div>
<div>　　1，不在work线程里面解析数据，直接在gui线程里面解析数据。</div>
<div>&nbsp;　　2，采用<a href="https://www.ibm.com/developerworks/cn/linux/thread/posix_threadapi/part2/" target="_blank">线程私有数据</a></div>


<p>（此处出现过，小插曲：QT的变量查看器，在调试时不太稳定，更新过后的值在窗口中查看，并没有得到及时更新，需要通过程序判定验证！）</p>


<p><img src="http://images2015.cnblogs.com/blog/781469/201603/781469-20160309170140991-312431222.png" alt="" /></p>


<p>由于程序需要在线程创建之后必须返回到gui主线程，所以采用创建两次线程的方式：创建线程A，返回，然后在线程A里面创建</p>
<p>在多线程POSIX标准下，主线程（1），即是GUI线程。它初始化一个类，并调用其一个成员函数创建一个线程3（ininlistenThred）【不知，为什么不从2开始？】，然后在线程3里面一直循环检测是否有新的客户端发来socket连接。当有新的客户端连接上时，即创建一个新的线程专门用于socket通信。</p>
<p>此时有一客户端连接上，创建了通信工作线程4（listenthreadwork）。</p>


<p>输出g_buf[0].carid的代码位置分别为：</p>
<p>1，initlistenthread线程在进入循环入口 即打印出：</p>
<div class="cnblogs_code">
<p>for( ; ; )<br />    {</p>
<p>　　 &nbsp;qDebug(<span style="color: #800000;">"</span><span style="color: #800000;">listenthread g_buf[0].carid: %s </span><span style="color: #800000;">"</span>,g_buf[<span style="color: #800080;">0</span><span>].carid);</span></p>
<p><em id="__mceDel"><span style="color: #0000ff;">&nbsp; 　　char</span> *t = &amp;(g_buf[<span style="color: #800080;">0</span>].carid[<span style="color: #800080;">0</span><span>]);</span></em></p>
<pre><em id="__mceDel"><em id="__mceDel"><span style="color: #000000;">　　qDebug(</span><span style="color: #800000;">"</span><span style="color: #800000;">dizhi: %d</span></em><span style="color: #800000;"> \n\n</span><span style="color: #800000;">"</span>,&amp;t);<br />//......<br />}</em></pre>
</div>
<p>2，gui主线程在点击按钮的时候，即打印：</p>
<div class="cnblogs_code">
<pre>qDebug(<span style="color: #800000;">"</span><span style="color: #800000;">mainwindow g_buf[0].carid: %s</span><span style="color: #800000;">"</span>,g_buf[<span style="color: #800080;">0</span><span style="color: #000000;">].carid);
</span><span style="color: #0000ff;">char</span> *t = &amp;(g_buf[<span style="color: #800080;">0</span>].carid[<span style="color: #800080;">0</span><span style="color: #000000;">]);
qDebug(</span><span style="color: #800000;">"</span><span style="color: #800000;">dizhi: %d \n\n</span><span style="color: #800000;">"</span>,&amp;t);</pre>
</div>
<p>3，listenthreadwork线程在循环工作的时候开始就打印信息：（一进去此线程就会先打印一次值，然后每次收到文件就会打印一次）</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">for</span><span style="color: #000000;">( ; ; )
    {
        qDebug(</span><span style="color: #800000;">"</span><span style="color: #800000;">listenthreadwork g_buf[0].carid: %s</span><span style="color: #800000;">"</span>,g_buf[<span style="color: #800080;">0</span><span style="color: #000000;">].carid);
        </span><span style="color: #0000ff;">char</span> *t = &amp;(g_buf[<span style="color: #800080;">0</span>].carid[<span style="color: #800080;">0</span><span style="color: #000000;">]);
        qDebug(</span><span style="color: #800000;">"</span><span style="color: #800000;">dizhi: %d \n\n</span><span style="color: #800000;">"</span>,&amp;<span style="color: #000000;">t);
</span><span style="color: #008000;">//</span><span style="color: #008000;">.........</span>
}</pre>
</div>
<p>&nbsp;</p>
<ul>
<li><strong>线程3和4的值一样的！</strong></li>
<li><strong>线程1跟另外两个线程是不同的值！</strong></li>
<li><strong>&nbsp;跟类没关系的两个线程值一样</strong></li>
</ul>
<p><img src="http://images2015.cnblogs.com/blog/781469/201603/781469-20160309171434179-548213179.png" alt="" /></p>
<p><img src="http://images2015.cnblogs.com/blog/781469/201603/781469-20160309191531804-530669003.jpg" alt="" /></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><img src="http://images2015.cnblogs.com/blog/781469/201603/781469-20160309191541944-1721815549.jpg" alt="" /></p>
<p>&nbsp;</p>



