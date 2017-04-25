+++ 
title = "macdown在mac OS 中的配置" 
date = "Wed, 02 Nov 2016 13:50:00 GMT" 
tags = ["配置"] 
categories = ["Tools"]
description = "用命令行打开markdown" 
+++ 

#### 配置方法
执行两条命令即可。

```
sudo echo "open -a MacDown \$*" > /usr/local/bin/macdown

sudo chmod +x /usr/local/bin/macdown
```

####  设置文件默认打开方式
- 选定文件
- 右击，选择显示简介
- 选择打开方式子选项
- 选择默认打开的程序，并设置全部更改。

#### 配置LaTeX语法显示
按如下配置即可预览LaTeX的公式。

<center>
<!-- ![](/images/macdown_show_Tex-like_math.png "示图1")
 -->
 <img src="/images/macdown_show_Tex-like_math.png" width = "420" height = "680" alt="示图1" align=center />

</center>




