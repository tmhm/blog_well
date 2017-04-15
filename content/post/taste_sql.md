+++
description = "初识SQL语言"
tags = ['SQL']
categories = ['language']
title = "SQL初识"
date = "2017-04-01T22:51:52+08:00"

+++

### 事务的四项特性

- ACID
- A（atomic）原子性
	- 用户a 少 200元－》 用户b 多 200元， 同时成立
	- 或者成立，或者回滚
	- 加锁，写日志	
- C（consistency）一致性
	- 事务不存在中间状态 
- I（isolation）隔离性
	- 一个事务没有完成，新事务提交请求，为了可以并发， 
	- undo－log记录行数据版本信息， 可重复读，
	- 序列化读，全部加锁
- D（durability） 持久性
	- 事务提交，记录到硬盘，
	- 刷新机制，时间－》秒， 事务提交即刷新，操作系统决定  

	
	

### SQL 
- 四种操作
- 增删改查
- ddl 数据定义语言，  建库，建表
- dml 数据操作语言，  增删改查
- dcl 数据控制语言，  用户授权，权限回收，清空表数据
- delete 会有redo－log， truncate 里面直接操作，不记日志

