+++ 
title = "ubuntu技巧" 
date = "Sat, 12 Mar 2016 03:34:00 GMT" 
tags = ["ubuntu"] 
categories = ["OS"]
description = "初入Ubuntu" 
+++ 



<ul>
<li><strong>在终端界面里面查找，命令：ctrl + shift +f</strong></li>
</ul>


<ul>
<li><strong>VMware 中 Ubuntu 不能全屏：</strong></li>
</ul>
<ol>
<li>转到虚拟机--&gt;安装VMware Tools</li>
<li>把装载的 VMware Tools CD里面的VMwareTools.x.x.x-xxxx.tar.gz文件解压到桌面</li>
<li>cd进入解压位置，运行&nbsp;sudo ./vmware-install.pl -d &nbsp;<span style="font-size: 13px;"><em>（注意：-d 开关假定您希望接受默认设置。如果不使用 -d，请按 Return 接受默认值或提供您自己的答案。）</em></span></li>
<li>重启</li>
<li>此时已经安装VMware tools，但是在ubuntu在VMware控制台显示时，四周仍然有黑框，可以在VMware中点击：查看--&gt;自动调整大小--&gt;自动适应客户机。</li>
</ol>
<ul>
<li><strong>&nbsp;codeblocks 编译开发环境(需要先安装gcc，g++,gdb)</strong>
<ul>
<li>添加头文件：依次点击project-&gt;bulid options-&gt;Search directories，在该标签页中点击Compiler，单击Add按钮添加头文件路径</li>
<li>添加静态库路径：依次点击project-&gt;bulid options-&gt;Linker setting，在该标签页中点击Add按钮添加静态库路径。</li>
<li>设置main函数参数：Project-&gt;Set programs&rsquo; arguments</li>
</ul>
</li>
<li>
<h3>&nbsp;查看ssh服务状态</h3>
</li>
<li>
<div class="cnblogs_code">
<pre>ps -e |grep ssh</pre>
</div>
<p>&nbsp;</p>
</li>
</ul>
<p>&nbsp;</p>



