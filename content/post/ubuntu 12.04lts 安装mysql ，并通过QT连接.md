+++ 
title = "ubuntu 12.04lts 安装 mysql，通过QT访问" 
date = "Thu, 18 Feb 2016 10:58:00 GMT" 
categories = ["ubuntu"] 
description = "在虚拟机 ubuntu 12.04lts 安装mysql，并通过QT连接。" 
+++ 


1. 安装server
`$ sudo apt-get install mysql-server`
2. 安装驱动`$ sudo apt-get install libqt4-sql-mysql`

3. 测试

```
$ dpkg --list | grep mysql</strong>
ii  libdbd-mysql-perl                           4.020-1build2                           Perl5 database interface to the MySQL database
ii  libmysqlclient18                            5.5.47-0ubuntu0.12.04.1                 MySQL database client library
ii  libqt4-sql-mysql                            4:4.8.1-0ubuntu4.9                      Qt 4 MySQL database driver
ii  mysql-client-5.5                            5.5.47-0ubuntu0.12.04.1                 MySQL database client binaries
ii  mysql-client-core-5.5                       5.5.47-0ubuntu0.12.04.1                 MySQL database core client binaries
ii  mysql-common                                5.5.47-0ubuntu0.12.04.1                 MySQL database common files, e.g. /etc/mysql/my.cnf
ii  mysql-server                                5.5.47-0ubuntu0.12.04.1                 MySQL database server (metapackage depending on the latest version)
ii  mysql-server-5.5                            5.5.47-0ubuntu0.12.04.1                 MySQL database server binaries and system database setup
ii  mysql-server-core-5.5                       5.5.47-0ubuntu0.12.04.1                 MySQL database server binaries
```

- 在目录 /usr/lib/i386-linux-gnu/qt4/plugins/sqldrivers/   找到文件 libqsqlmysql.so  
复制到 qt的安装目录下路径： /usr/local/Trolltech/Qt-4.8.6/plugins/sqldrivers/  


```
sudo cp libqsqlmysql.so /usr/local/Trolltech/Qt-4.8.6/plugins/sqldrivers/

```

- 确定具有x属性

```
-rwxr-xr-x 1 tl tl 533748 8月 9 2012 libqsqlite.so
-rwxr-xr-x 1 tl tl 67816 5月 28 2015 libqsqlmysql.so
```

<p><strong>测试结果：</strong></p>
<p><img src="http://images2015.cnblogs.com/blog/781469/201602/781469-20160218185633503-1658976193.jpg" alt="" />　　</p>
<p>&nbsp;</p>



