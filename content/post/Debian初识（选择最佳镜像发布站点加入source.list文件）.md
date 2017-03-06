+++ 
title = "Debian初识（选择最佳镜像发布站点加入source.list文件）" 
date = "Tue, 08 Mar 2016 07:28:00 GMT" 
tags = ["Linux"] 
categories = ["OS"]
description = "镜像源source.list配置" 
+++ 

#### 选择最佳镜像发布站点加入source.list文件：netselect，netselect-apt


该将哪个Debian镜像发布站点加入<samp>source.list</samp>文件？&rdquo;。有很多方法来选择镜像发布站点，专家们可能会写一个脚本去测试 不同站点的ping时间。不过其实有一个程序可以帮你：&nbsp;<strong>netselect</strong>。

要安装netselect，通常使用：

```
# apt-get install netselect

# sudo netselect ftp.debian.org http.us.debian.org ftp.at.debian.org download.unesp.br ftp.debian.org.br ftp.hk.debian.org ftp.tw.debian.org debian.linux.org.tw ftp.sjtu.edu.cn mirrors.163.com
  150 mirrors.163.com
```
<p>表示，在netselect后列出的所有主机中，mirrors.163.com是下载速 度最快的主机，其得分为150。(注意！！这是在我电脑上的测试结果，不同的网络 节点网速会大不相同，所以这个分值不一定适用于其它电脑)</p>
<div class="cnblogs_code">
<pre># apt-<span style="color: #0000ff;">get</span> install netselect-apt<br /># netselect-apt stable</pre>
</div>
<p>从0.3.ds1版开始，netselect源码包中包含了<strong>netselect-apt</strong>二 进制包，它使上述操作自动完成。只需将发布目录树做为参数(默认为stable)输入，&nbsp;<samp>sources.list</samp>文件就会生成速度最快的main和non-US镜像站点列表，并保 存在当前目录下。下面的例子生成一个包含stable发布镜像站点列表的sources.list：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;"> # ls sources.list
     ls: sources.list: File or directory not found
 # netselect</span>-<span style="color: #000000;">apt stable
     (...)
 # ls </span>-<span style="color: #000000;">l sources.list
     sources.list
 #</span></pre>
</div>
<p><strong>记住：</strong><samp>sources.list</samp>生成在当前目录下，必须将其移 至<samp>/etc/apt</samp>目录。 根据文档内提示：打开注释deb-src。</p>


<ul>
<li><strong>putty在win10下面访问不了debian8.2，可能原因有：</strong></li>
</ul>
<p>　　　　1，防火墙</p>
<p>　　　　2，网卡驱动不支持！！这点需注意，升级电脑，被坑！</p>
<ul>
<li><strong>&nbsp;查看软件安装位置</strong></li>
<li>
<div class="cnblogs_code">
<pre>whereis torcs</pre>
<p>man whereis</p>
<p>NAME<br />       whereis  -  locate the binary, source, and manual page files for a com‐<br />       mand</p>

</div>



</li>

</ul>
<ul>
<li><strong>查看软件运行位置</strong>
<div class="cnblogs_code">
<pre>which torcs  </pre>
<p>man which<br />NAME<br />       which - locate a command</p>

</div>


</li>

</ul>





