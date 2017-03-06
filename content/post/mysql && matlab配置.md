+++ 
title = "mysql && matlab配置" 
date = "Sat, 21 Nov 2015 13:56:00 GMT" 
tags = ["配置"] 
categories = ["Tools"]
description = " Mysql 和 Matlab 的 连接配置入门。" 
+++ 


#### mysql 中一直出现'&gt;

>单双引号没有配对
	
#### mysql 连接matlab
- 1， 到mysql官网下载 http://dev.mysql.com/downloads/connector/j/<br />(mysql-connector-java-5.1.37.tar.gz<br />将mysql-connector-java-5.1.37-bin.jar  放在matlabroot/java/jar/toolbox/
- 2， 在E:\Program Files\MATLAB\R2011b\toolbox\local目录下，找到classpath.txt<br />增加一句<br />$matlabroot/java/jar/toolbox/mysql-connector-java-5.1.37-bin.jar
	
	注意：驱动名字匹配; matlabroot指安装目录
- 3，重启matlab
- 4，测试</p>

	```	
	conn=database('xw','root','key','com.mysql.jdbc.Driver','jdbc:mysql://localhost:3306/xw')
	aTemp = exec(conn,'select * from gd_train_data limit 5
	
	aTemp = fetch(aTemp)
	curs = aTemp.Data
	
	```


