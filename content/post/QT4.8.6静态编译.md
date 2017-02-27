+++ 
title = "QT4.8.6静态编译" 
date = "Tue, 08 Mar 2016 12:48:00 GMT" 
categories = ["编译"] 
description = " 配置QT静态编译" 
+++ 

<ul>
<li><strong>下载源安装程序，http://download.qt.io/archive/qt/4.8/4.8.6/qt-everywhere-opensource-src-4.8.6.tar.gz</strong></li>
<li><strong>解压</strong></li>
<li><strong>cd 进入解压后的目录，命令</strong></li>
</ul>


<div class="cnblogs_Highlighter">
<pre class="brush:cpp;gutter:true;">./configure -static -release -opensource -qt-zlib -qt-libpng -qt-libmng -qt-libjpeg -nomake demos -nomake examples -qt-sql-sqlite -prefix /usr/local/Trolltech/Qt-4.8.6_static</pre>
</div>
<p>命令参数含义如下所示（带有+号的是默认选项）：</p>
<div class="cnblogs_Highlighter">
<pre class="brush:cpp;gutter:true;">-static ............ Create and use static Qt libraries.
-opensource ........ Compile and link the Open-Source Edition of Qt.
-release ........... Compile and link Qt with debugging turned off.


Third Party Libraries:

-qt-zlib ........... Use the zlib bundled with Qt.
+ -system-zlib ....... Use zlib from the operating system.
See http://www.gzip.org/zlib

-no-gif ............ Do not compile GIF reading support.

-no-libtiff ........ Do not compile TIFF support.
-qt-libtiff ........ Use the libtiff bundled with Qt.
+ -system-libtiff .... Use libtiff from the operating system.
See http://www.libtiff.org

-no-libpng ......... Do not compile PNG support.
-qt-libpng ......... Use the libpng bundled with Qt.
+ -system-libpng ..... Use libpng from the operating system.
See http://www.libpng.org/pub/png

-no-libmng ......... Do not compile MNG support.
-qt-libmng ......... Use the libmng bundled with Qt.
+ -system-libmng ..... Use libmng from the operating system.
See http://www.libmng.com

-no-libjpeg ........ Do not compile JPEG support.
-qt-libjpeg ........ Use the libjpeg bundled with Qt.
+ -system-libjpeg .... Use libjpeg from the operating system.
See http://www.ijg.org

-no-sql-&lt;driver&gt; ... Disable SQL &lt;driver&gt; entirely.
-qt-sql-&lt;driver&gt; ... Enable a SQL &lt;driver&gt; in the QtSql library, by default
none are turned on.
-plugin-sql-&lt;driver&gt; Enable SQL &lt;driver&gt; as a plugin to be linked to at run time.
Possible values for &lt;driver&gt;:
[ db2 ibase mysql oci odbc psql sqlite sqlite2 sqlite_symbian symsql tds ]

-prefix &lt;dir&gt; ...... This will install everything relative to &lt;dir&gt;
(default /usr/local/Trolltech/Qt-4.8.6_static)</pre>
</div>
<ul>
<li><strong>make</strong></li>
</ul>
<p style="margin-left: 60px;">make的时间还是那么的长。。。</p>
<p style="margin-left: 90px;">在configure命令的参数里面，没有添加-qt-sql-sqlite的时候， 会出现下面错误。</p>
<p style="margin-left: 90px;">&nbsp;</p>
<p style="margin-left: 90px;">只好重新configure了。</p>
<div class="cnblogs_Highlighter" style="margin-left: 30px;">
<pre class="brush:cpp;gutter:true;">main.cpp:(.text.startup+0x1dd6): undefined reference to `qt_plugin_instance_qsqlite()'
collect2: ld 返回 1
make[4]: *** [../../../../bin/assistant] 错误 1
make[4]:正在离开目录 `/media/software/qtRes/qt-everywhere-opensource-src-4.8.6/tools/assistant/tools/assistant'
make[3]: *** [sub-assistant-make_default-ordered] 错误 2
make[3]:正在离开目录 `/media/software/qtRes/qt-everywhere-opensource-src-4.8.6/tools/assistant/tools'
make[2]: *** [sub-tools-make_default-ordered] 错误 2
make[2]:正在离开目录 `/media/software/qtRes/qt-everywhere-opensource-src-4.8.6/tools/assistant'
make[1]: *** [sub-assistant-make_default-ordered] 错误 2
make[1]:正在离开目录 `/media/software/qtRes/qt-everywhere-opensource-src-4.8.6/tools'
make: *** [sub-tools-make_default-ordered] 错误 2
</pre>
</div>
<ul>
<li><strong>sudo make install</strong></li>
<li><strong>测试：</strong></li>
</ul>
<ol>
<li>建立文件夹，然后写程序文件main.cpp</li>
<li>qmake -project</li>
<li>生成pro文件后在里面加入CONFIG += static</li>
<li>qmake</li>
<li>生成Makefile后在cxxflags的=后插入-static</li>
<li>make</li>
</ol>
<p>&nbsp;</p>



