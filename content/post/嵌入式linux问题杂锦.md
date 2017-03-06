+++ 
title = "嵌入式linux问题杂锦" 
date = "Fri, 08 Jan 2016 08:35:00 GMT" 
tags = ["Linux"] 
categories = ["OS"]
description = "嵌入式 Linux 系统的小问题记录。 " 
+++ 


<li><strong><strong>tftp 在开发板上不能获取共享文件，出现：</strong></strong>&nbsp;Permission denied</li>
</ul>
<div class="cnblogs_code">
</div>
<ul>
<li>
<p>是因为，我在/sys目录下执行的，</p>
</li>
</ul>
<div class="cnblogs_Highlighter">
<pre class="brush:cpp;gutter:true;">root@am437x-evm:/sys# tftp -g -r myTcpTest 172.20.9.59
tftp: can't open 'myTcpTest': Permission denied
</pre>
</div>
<p>　　回到～ 主文件夹，既可以获取：</p>
<div class="cnblogs_code">
<pre>root@am437x-evm:~# tftp -g -r myTcpTest <span style="color: #800080;">172.20</span>.<span style="color: #800080;">9.59</span> </pre>
</div>





