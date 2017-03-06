+++ 
title = "C_Cplus中一些不太注意到的小知识点--[锦集]" 
date = "Sun, 22 May 2016 14:55:00 GMT" 
tags = ["C++"] 
categories = ["Language"]
description = " inside 运算符和sizeof。" 
+++ 

C/C++小知识点--[锦集]

####  “=”和“<=” 的优先级

1.(   (file_got_len = recv_str(sock,buf,BUF_SIZE) )    <= 0)

2.(     file_got_len = recv_str(sock,buf,BUF_SIZE) <= 0   )

第二种情况下，当recv_str()函数成功返回发送字符串的时候，尽管会成功返回发送字节数大于0，

但是，file_got_len只会返回0，

因为“<=”的优先级大于“=”。

所以在多语句写在一起时，最好用（） 明示！！


####  运算操作符

```
int i,t;
t = (i=1)+(++i);
```
输出
`t= 4
`

i首先被赋值1，随后++i，使i的值变为2，到做加法的操作的时候，两个操作数都要读i此时的值，结果是两个2相加，等于4.

其汇编程序（基于vs2010），如下：

```
int i,t;
t = (i=1)+(++i);
011413FE  mov         dword ptr [i],1
01141405  mov         eax,dword ptr [i]
01141408  add         eax,1
0114140B  mov         dword ptr [i],eax
0114140E  mov         ecx,dword ptr [i]
01141411  add         ecx,dword ptr [i]
01141414  mov         dword ptr [t],ecx

```
i自加后，只保留在eax 寄存器里，然后将eax的2赋给i，所以，i 就变成2了。此处和动态语言的引用似乎有所类似。

####  sizeof操作符与数组

```
int t,tt,num;
int array[5];
int *p = array;

t= sizeof(p); //t =4
t= sizeof(int [5]);     // t= 20
tt = sizeof(array);     // tt = 20
num = sizeof(array)/sizeof(array[0]);  // get the number of array .  num =5
```
开始自以为，数组名不是首地址嘛。 应该是一个指针，然后对指针sizeof ，应该是4
个字节才对，**为什么呢？**

此时**sizeof的是整个数组，而非首指针**。
要学会多查看[MSDN](https://msdn.microsoft.com/en-us/library/0w557fh7.aspx)，解释如下：
> When you apply the sizeof operator to an array identifier, the result is the size of the entire array rather than the size of the pointer represented by the array identifier.



