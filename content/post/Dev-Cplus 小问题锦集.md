+++ 
title = "Dev-C 小问题锦集" 
date = "Sun, 21 Feb 2016 13:10:00 GMT" 
tags = ["C++"] 
categories = ["Tools"]
description = " C++ 无法调试" 
+++ 


- C++ project cann't debug

```
Your project does not have debugging information, do you want to enable debugging and rebuild your project?

```

解决办法：

在tools 菜单下的compiler项中，勾选：`Add the following commands when calling compiler: `
<p>并在框内添加参数：-g</p>
<p>（版本：Dev-C++ 4.9.9.2.）</p>




