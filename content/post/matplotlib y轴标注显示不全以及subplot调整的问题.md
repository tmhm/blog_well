+++ 
title = "matplotlib y轴标注显示不全以及subplot调整的问题" 
date = "Fri, 23 Dec 2016 08:32:00 GMT" 
tags = ["Python"] 
categories = ["Tools"]
description = "在Python中使用matplotlib调整位置大小的代码" 
+++ 


问题：
我想在y轴显示的标注太长，想把它变成两行显示，发现生成的图形只显示的第二行的字，把第一行的字**挤出去**了

想要的是**显示两行**这样子的

![](http://i.imgur.com/6fxUwHR.jpg)

现实却是这样子

![](http://i.imgur.com/XZmJtEF.jpg)

#### 主要相关的api有：
- plt.subplots_adjust
- set_ylabel
- plt.text

####  解决思路
来自matplotlib的[官网](http://matplotlib.org/gallery.html),以及[图示](http://matplotlib.org/examples/images_contours_and_fields/interpolation_none_vs_nearest.html)

1. 当出现右边显示不全的时候，第一感觉是：应该可以通过set_ylabel 来设置y轴标注的位置和大小，发现set_ylabel只能设置labelpad ，没有位置的参数；
2. 然后找到了可以用text设置标注字体的位置和方向，但是要多次定位，尝试，发现不方便；
2. 那么想到的是应该可以设置plot的位置吧，用ax1.plot?找了下，也没看到，不爽；
3. 在show的图形界面发现一个移动的按钮![](http://i.imgur.com/VThZrvx.jpg)
4. 移动left亦可以显示出y轴的标注了，那么我应该只需要在程序中设置一下left的参数既可以了吧，在上面[matplotlib的example]()中，找到了我想要的subplots_adjust
5. 即有了下面的源代码，满足设置要求。

源代码如下：

```
import matplotlib.pyplot as plt
import numpy as np
from numpy import abs

size=[5,10,20,30,50,100]
avg=[-0.2896,0.073865632,0.034858287,-0.092241705,-0.022924236,0.016541661]
avr=[0.032,0.077757872,0.090351641,0.036522663,0.034413038,0.096587464]

fig = plt.figure()

ax1 = fig.add_subplot(211)
lns1 = ax1.plot(size, trackPos_avg,color='blue',label='error average',linestyle='-',linewidth=1.9)
ax1.set_ylabel('deviation from\ncentral line ($m$)',fontsize=18, labelpad = 0.5)

plt.xticks(fontsize = 17)#对坐标的值数值，大小限制
plt.yticks(fontsize = 17)

ax2 = fig.add_subplot(212)
ax2.set_ylabel('standard \nvariance ($m^2$)',fontsize=18,labelpad = 12.5)
lns2 = ax2.plot(size, avr, color='red',label='mean square error',linestyle='-',linewidth=1.9)

plt.xticks(fontsize = 17)#对坐标的值数值，大小限制
plt.yticks(fontsize = 17)
ax2.set_xlabel('replay size',fontsize=18)

plt.subplots_adjust(left=0.18, wspace=0.25, hspace=0.25,
                    bottom=0.13, top=0.91)

\#plt.text(0.4, 0.4, 'deviation from\n central line ($m$)', rotation=90, ha='left')

\#plt.legend(prop={'size':18})  # loc='upper left',

\#fig.savefig('./figure/error_paper.eps', format='eps', dpi=1000)
fig.savefig('./figure/error_paper.png', dpi=1000)

plt.show()
```

可以下面的yticks，限制横纵坐标的值

```
plt.xticks(fontsize = 17)#对坐标的值数值，大小限制
plt.yticks([0.01,0.03,0.05,0.07,0.09],fontsize = 17)
```


