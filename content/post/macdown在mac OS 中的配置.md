+++ 
title = "macdown在mac OS 中的配置" 
date = "Wed, 02 Nov 2016 13:50:00 GMT" 
categories = ["配置"] 
description = "用命令行打开markdown" 
+++ 


执行两条命令即可。

```
sudo echo "open -a MacDown \$*" > /usr/local/bin/macdown

sudo chmod +x /usr/local/bin/macdown
```

###  设置文件默认打开方式
- 选定文件
- 右击，选择显示简介
- 选择打开方式子选项
- 选择默认打开的程序，并设置全部更改。



