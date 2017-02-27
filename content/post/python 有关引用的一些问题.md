+++ 
title = "python 有关引用的一些问题" 
date = "Sat, 21 May 2016 14:24:00 GMT" 
categories = ["python"] 
description = "Python中引用的一些分析记录" 
+++ 


```
print id.__doc__
​
id(object) -> integer

Return the identity of an object.  This is guaranteed to be unique among
simultaneously existing objects.  (Hint: it's the object's memory address.)
```

####  python中的引用对象特点：
- python不允许程序员选择采用传值还是传引用。
        - Python参数传递采用的肯定是“传对象引用”的方式。实际上，这种方式相当于传值和传引用的一种综合。如果函数收到的是一个可变对象（比如字典或者列表）的引用，就能修改对象的原始值——相当于通过“传引用”来传递对象。
        - 如果函数收到的是一个不可变对象（比如数字、字符或者元组）的引用，就不能直接修改原始对象——相当于通过“传值'来传递对象。
- 当人们复制列表或字典时，就复制了对象列表的引用，如果改变引用的值，则修改了原始的参数。
- 为了简化内存管理，Python通过引用计数机制实现自动垃圾回收功能，Python中的每个对象都有一个引用计数，用来计数该对象在不同场所分别被引用了多少次。每当引用一次Python对象，相应的引用计数就增1，每当消毁一次Python对象，则相应的引用就减1，只有当引用计数为零时，才真正从内存中删除Python对象。
[参考](http://www.cnblogs.com/yuyan/archive/2012/04/21/2461673.html)

以下是一些例子：


In [19]:

```
# variable 动态创建一个新的变量，但是，list，tuple，dictionary 却不会创建新的实例
a= 1
b = a
print id(a)
print id(b)
​
b = 3
print "change b to 3 "
print  "a: %s, b: %s" %(a ,b)
print "a id is : %s; b id is : %s" %(id(a),id(b))
```

out:

```
42295960
42295960
change b to 3
a: 1, b: 3
a id is : 42295960; b id is : 42295912
```

In [25]:

```
# list
a= [1,2,3]
b = a   # 引用
print id(a)
print id(b)
​
b[2] = 6
print "change b to 3 "
print  "a: %s, b: %s" %(a ,b)
print "a id is : %s; b id is : %s" %(id(a),id(b))
```

out：

```
66189960
66189960
change b to 3
a: [1, 2, 6], b: [1, 2, 6]
a id is : 66189960; b id is : 66189960
```

In [38]:

```
# list
a= [1,2,3]
b = a[:] # 值拷贝， 创建了新的对象实例
print id(a)
print id(b)
​
b[2] = 6
print "change b to 3 "
print  "a: %s, b: %s" %(a ,b)
print "a id is : %s; b id is : %s" %(id(a),id(b))
```
out：

```
65422344
65421832
change b to 3
a: [1, 2, 3], b: [1, 2, 6]
a id is : 65422344; b id is : 65421832
```

In [32]:

```
# dictionary
a= {'ta':11,'tb':22,'tc':33}
b = a # 引用，改变的是原实例的值
print id(a)
print id(b)
​
b['tb'] = 6
print "change b to 3 "
print  "a: %s,\n b: %s" %(a ,b)
print "a id is : %s;\n b id is : %s" %(id(a),id(b))
```

out：

```
66214904
66214904
change b to 3
a: {'tb': 6, 'tc': 33, 'ta': 11},
 b: {'tb': 6, 'tc': 33, 'ta': 11}
a id is : 66214904;
 b id is : 66214904
```

In [36]:

```
# tuple 元组用"()"标识。内部元素用逗号隔开。但是元组不能二次赋值，相当于只读列表。
a= (1,2,3)
b = a
print id(a)
print id(b)
​
# b[0] = 6
print "change b to 3 "
print  "a: %s,\n b: %s" %(a ,b)
print "a id is : %s;\n b id is : %s" %(id(a),id(b))
```

out：

```
66132296
66132296
change b to 3
a: (1, 2, 3),
 b: (1, 2, 3)
a id is : 66132296;
 b id is : 66132296
```

In [9]:此案例的问题来自[博文](http://www.cnblogs.com/yuyan/archive/2012/04/21/2461673.html) ，经改进。如下

```
def add_list(p):
    pt = p +[5,6]  # 1
    p = p + [5,6]  # 2   1 和2 是等价的，没有将值返回， ‘=’左边的变量，都是函数内部生成的局部临时对象，并没有返回，故不会修改传入参数的值。
    # 此处和静态语言的理解方式是一样的。
​
p1 = [1,2,3]
add_list(p1)  #1 和2 是等价的，没有改变返回值
print p1
​
​
def add_list2(p):
    p += [5,6]
p2 = [1,2,3]
add_list2(p2)
print p2
```

out：​

```
[1, 2, 3]
[1, 2, 3, 5, 6]
```

此处‘=’号左边的p 应该是一个函数新建的一个局部的、临时的对象实例，等号的右边的p是才是函数传进来的，由于临时的“P”并没有返回，故肯定不会改变传入list的值。此处和静态语言应该是一致的。 所以，它并没有修改原来的p引用。


In [6]:

```
help('+=')

An augmented assignment expression like "x += 1" can be rewritten as
"x = x + 1" to achieve a similar, but not exactly equal effect. In the
augmented version, "x" is only evaluated once. Also, when possible,
the actual operation is performed *in-place*, meaning that rather than
creating a new object and assigning that to the target, the old object
is modified instead.
```


In [11]:

```
# 字典引用
a = []
b = {'num':0, 'sqrt':0}
resurse = [1,2,3]
for i in resurse:
  b['num'] = i
  b['sqrt'] = i * i
  a.append(b)
print "a: ",a
​
d=[]
for i in resurse:
    b['num'] = i
    b['sqrt'] = i * i
    d1 = b.copy()
    d.append(d1)
print "d: ",d
​
c=[]
for i in resurse:
    c.append({"num":i, "sqrt":i*i})
print "c: ",c
```

out：

```
​a:  [{'num': 3, 'sqrt': 9}, {'num': 3, 'sqrt': 9}, {'num': 3, 'sqrt': 9}]
d:  [{'num': 1, 'sqrt': 1}, {'num': 2, 'sqrt': 4}, {'num': 3, 'sqrt': 9}]
c:  [{'num': 1, 'sqrt': 1}, {'num': 2, 'sqrt': 4}, {'num': 3, 'sqrt': 9}]
```

b['num'] = i  和 b['sqrt'] = i * i  中的 b['num'] 和 b['sqrt'] 是已经**压入list a 中元素的一个引用**，故它可以在不断地改变list 内部变量的值。

单步调试可以看到，

a中值的变化情况：*以执行完语句a.append(b)为节点 *

[{'num': 1, 'sqrt': 1}]

--> [{'num': 2, 'sqrt': 4},{'num': 2, 'sqrt': 4}]

-->  [{'num': 3, 'sqrt': 9}, {'num': 3, 'sqrt': 9}, {'num': 3, 'sqrt': 9}]

在append(b)到list a之前获得b的值拷贝，将值append 到list a 也可以达到目标。如**示例d**所示。

​
当然，示例C是更加简洁的一个版本，这里应该还有迭代器的知识点，暂时还没折腾内部，待到下次和生成器一起分析。



