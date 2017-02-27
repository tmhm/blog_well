+++ 
title = "QT creator 调试问题" 
date = "Fri, 08 Jan 2016 07:45:00 GMT" 
categories = ["QT"] 
description = " QT Creator 调试小问题记录" 
+++ 

- 问题：debug出现&ldquo;ptrace：不允许的操作。

解决办法：

```
# may not be appropriate for developers or servers with only admin accounts.
kernel.yama.ptrace_scope = 1
```
<p>修改为：</p>

```
kernel.yama.ptrace_scope = 0
```

<p>然后终端输入，重启：</p>
<div class="cnblogs_code">
<pre>sudo reboot</pre>
</div>
<p>完毕。</p>



- 问题：During startup program exited with code 126.


<p><img style="line-height: 1.5;" src="http://images2015.cnblogs.com/blog/781469/201601/781469-20160111204605007-2048896612.jpg" alt="" /></p>
<p>原因：编译的是基于嵌入式开发板的二进制文件，在ubuntu上运行不了，放到开发板上能正常运行。</p>



<li><strong>问题：打开Qt creator出现&ldquo;无法写入文件 qtversion.xml.磁盘已满？&rdquo;</strong></li>
</ul>
<p><img src="http://images2015.cnblogs.com/blog/781469/201601/781469-20160115153807366-1882790920.jpg" alt="" /></p>
<p>原因：权限不够</p>
<div class="cnblogs_Highlighter">
<pre class="brush:cpp;gutter:true;">-rwx--x--x  1 tl tl  1592  1月 15 15:30 qtversion.xml
</pre>
</div>
<p>解决办法：添加读写权限</p>
<p>　　</p>




