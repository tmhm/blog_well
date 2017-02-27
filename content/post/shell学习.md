+++ 
title = "shell学习" 
date = "Thu, 10 Mar 2016 08:36:00 GMT" 
categories = ["Linux"] 
description = "shell小记" 
+++ 

 
- readlink是linux系统中一个常用工具，主要用来找出符号链接所指向的位置。
- 定义变量“=”两边不能有空格，否则会被shell解析错误。
- tee 同时将输出内容显示在屏幕上、记录在文件里
- 三种引号‘’，“”，``。
	- rm -rf ‘my document’  ----当命令行里面包含空格时，用单引号包含起来
	- 这时也可由用双引号，区别在于双引号的值会被变量的之替换，单引号保持原样。
 
 	```
	$ ABC=hello
	$ echo 'string is ${ABC}'
	string is ${ABC}
	$ echo "string is ${ABC}"
	string is hello
	```
	
	反引号实际上就是命令替换
	
	```
	echo `uname`  #等价 echo $(uname)
	$ echo `uname -a`
	Linux ubuntu 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
	$ uname -a
	Linux ubuntu 3.13.0-24-generic #46-Ubuntu SMP Thu Apr 10 19:11:08 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
 	```

#### 脚本的执行

	
<div class="cnblogs_code">
<pre><span style="color: #000000;">$ vi test.sh
$ cat test.sh
echo </span><span style="color: #800000;">"</span><span style="color: #800000;"> this is a test</span><span style="color: #800000;">"</span><span style="color: #000000;">
	
$ ls </span>-<span style="color: #000000;">al
total </span><span style="color: #800080;">12</span><span style="color: #000000;">
drwxr</span>-xr-x  <span style="color: #800080;">2</span> xwei xwei <span style="color: #800080;">4096</span> Mar <span style="color: #800080;">10</span> <span style="color: #800080;">16</span>:<span style="color: #800080;">49</span><span style="color: #000000;"> .
drwxr</span>-xr-x <span style="color: #800080;">16</span> xwei xwei <span style="color: #800080;">4096</span> Mar <span style="color: #800080;">10</span> <span style="color: #800080;">15</span>:<span style="color: #800080;">28</span><span style="color: #000000;"> ..
</span>-rw-rw-r--  <span style="color: #800080;">1</span> xwei xwei   <span style="color: #800080;">26</span> Mar <span style="color: #800080;">10</span> <span style="color: #800080;">16</span>:<span style="color: #800080;">49</span><span style="color: #000000;"> test.sh
$ . test.sh  #以source test.sh 方式执行，读入文件中的命令，在当前shell执行，source内置命令，无需执行权限，其可以缩写为一个小数点
 </span><span style="color: #0000ff;">this</span> <span style="color: #0000ff;">is</span><span style="color: #000000;"> a test
$ .</span>/<span style="color: #000000;">test.sh  #
bash: .</span>/<span style="color: #000000;">test.sh: Permission denied
	
xwei@ubuntu:</span>~/Desktop$ chmod a+<span style="color: #000000;">x test.sh
xwei@ubuntu:</span>~/Desktop$ ls -<span style="color: #000000;">al
total </span><span style="color: #800080;">12</span><span style="color: #000000;">
drwxr</span>-xr-x  <span style="color: #800080;">2</span> xwei xwei <span style="color: #800080;">4096</span> Mar <span style="color: #800080;">10</span> <span style="color: #800080;">16</span>:<span style="color: #800080;">49</span><span style="color: #000000;"> .
drwxr</span>-xr-x <span style="color: #800080;">16</span> xwei xwei <span style="color: #800080;">4096</span> Mar <span style="color: #800080;">10</span> <span style="color: #800080;">15</span>:<span style="color: #800080;">28</span><span style="color: #000000;"> ..
</span>-rwxrwxr-x  <span style="color: #800080;">1</span> xwei xwei   <span style="color: #800080;">26</span> Mar <span style="color: #800080;">10</span> <span style="color: #800080;">16</span>:<span style="color: #800080;">49</span><span style="color: #000000;"> test.sh
$ .</span>/<span style="color: #000000;">test.sh  #在一个子shell中执行命令
 </span><span style="color: #0000ff;">this</span> <span style="color: #0000ff;">is</span> a test</pre>
</div>
<p>&nbsp;</p>
</li>
</ul>
</li>
</ul>


	
	
	
