+++ 
title = "python 与时间有关的操作" 
date = "Tue, 31 May 2016 10:46:00 GMT" 
categories = ["python"] 
description = "time 模块" 
+++ 

源代码：

```
import time
# ISOTIMEFORMAT='%Y-%m-%d %X'
ISOTIMEFORMAT='%Y-%m-%d'
t= time.strftime( ISOTIMEFORMAT, time.gmtime( time.time() ) )
f = open( t + "test.txt",'wb')
f.write("hello,\n This is a test file")
f.close()
```

###  计算程序运行时间

```
import time
# ISOTIMEFORMAT='%Y-%m-%d %X'
start = time.time()
ISOTIMEFORMAT='%Y-%m-%d'
t= time.strftime( ISOTIMEFORMAT, time.gmtime( time.time() ) )
f = open( t + "test.txt",'wb')
f.write("hello,\n This is a test file")
f.close()
print t
elapsed = time.time() - start
print elapsed
```

out：
​
```
2016-05-31
0.00300002098083
```


