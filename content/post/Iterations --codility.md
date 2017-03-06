+++ 
title = "Iterations --codility" 
date = "Thu, 01 Sep 2016 15:35:00 GMT" 
tags = ["codility"] 
categories = ["Algorithm"]
description = "迭代刷题" 
+++ 

<h3 id = "1.1">
lesson 1：<i>Iterations</i>
</h3>

<h4 id = "1.1.1">
1. BinaryGap-----[100%]
</h4>

> Find longest sequence of zeros in binary representation of an integer.

A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.

For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps.


解题思路：

- 将整型数据转为二进制，然后利用python的split函数按‘1’分割，再用max函数找到最大的切片即可
- 注意二进制为全1，或者第一位为1，后面全为0的数，e.g. 1, 100, 11111, 用log次的循环判定下即可
- 满足于O(log(N))的时间复杂度


```
from math import log
def solution(N):

    #if N == 1:   #can belong to the next for i = 0
    #    return 0
    for i in xrange(int(log(N,2))+1):

        tmp = 2**i
        #print tmp
        if tmp == N+1 or tmp == N:
            return 0

    strb = bin(N).split("1")
    #print strb[1:-1]
        #last one can't included. e.g.1001000->2, so [1:-1]
    if strb[1:-1] is None:
        #print "test"
        return 0
    return max([len(l) for l in strb[1:-1]])
```



