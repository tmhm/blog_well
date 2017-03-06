+++ 
title = "python一些内建函数(map,zip,filter,reduce,yield等)" 
date = "Tue, 17 May 2016 15:06:00 GMT" 
tags = ["python"] 
categories = ["Language"]
description = "Python中一些有用的内置函数，map，zip，filter，reduce，yield，instance等" 
+++ 


#### map函数
Python实际上提供了一个内置的工具，map函数。这个函数的主要功能是对一个序列对象中的每一个元素应用被传入的函数，并且返回一个包含了所有函数调用结果的一个列表。

map?

```
Docstring:
map(function, sequence[, sequence, ...]) -> list

Return a list of the results of applying the function to the items of
the argument sequence(s).  If more than one sequence is given, the
function is called with an argument list consisting of the corresponding
item of each sequence, substituting None for missing values when not all
sequences have the same length.  If the function is None, return a list of
the items of the sequence (or a list of tuples if more than one sequence).
Type:      builtin_function_or_method
```

下面三个例子参考了[该博文](http://my.oschina.net/zyzzy/blog/115096)
​
对可迭代函数'iterable'中的每一个元素应用‘function’方法，将结果作为list返回。

```
def add100(x):
    return x+100
hh = [11,22,33]
map(add100,hh)
Out[2]:
[111, 122, 133]
```

如果给出了额外的可迭代参数，则对每个可迭代参数中的元素‘并行’的应用‘function’。

```
def abc(a, b, c):
    return a*10000 + b*100 + c
​
list1 = [11,22,33]
list2 = [44,55,66]
list3 = [77,88,99]
map(abc,list1,list2,list3)
Out[3]:
[114477, 225588, 336699]
```

如果'function'给出的是‘None’，自动假定一个‘identity’函数（这个‘identity’不知道怎么解释，看例子吧）

```
list1 = [11,22,33]
map(None,list1)
Out[5]:
[11, 22, 33]


list1 = [11,22,33]
list2 = [44,55,66]
list3 = [77,88,99]
map(None,list1,list2,list3)
Out[6]:
[(11, 44, 77), (22, 55, 88), (33, 66, 99)]
```

#### zip函数
以下参考了[博文](http://www.bkjia.com/Pythonjc/664161.html)

函式说明：zip(seq1[,seq2 [...]])->[(seq1(0),seq2(0)...,(...)]。 同时循环两个一样长的函数，返回一个包含每个参数元组对应元素的元组。若不一致，采取截取方式，使得返回的结果元组的长度为各参数元组长度最小值。

```
for x,y in zip([1,2,3],[4,5,6]):
    print x,y
for x,y in zip([1,2,3],[4,5,6]):
    print x,y
1 4
2 5
3 6


for x,y in zip([1,2,3],[5,6]):
    print x,y
1 5
2 6
        
```
#### filter函数
filter(bool_func,seq)：此函数的功能相当于过滤器。 调用一个布尔函数bool_func来迭代遍历每个seq中的元素，返回一个使bool_seq返回值为true的元素的序列。

```
filter(lambda x:x%2==0,[1,2,3,4,6,5,7])
Out[15]:
[2, 4, 6]   
```

#### reduce函数

reduce(func,seq[,init])：func为二元函数，将func作用于seq序列的元素，每次携带一对（先前的结果以及下一个序列的元素），连续的将现有的结果和下一个值作用在获得的随后的结果上，最后减少我们的序列为一个单一的返回值：如果初始值init给定，第一个比较会是init和第一个序列元素而不是序列的头两个元素。

```
reduce(lambda x,y:x+y,[1,2,3,4])
Out[22]:
10

reduce(lambda x,y: x-y,[1,2,3,4,5],19)
reduce(lambda x,y: x-y,[1,2,3,4,5],19)
Out[23]:
4

reduce(lambda x,y: x-y,[1,2,3,5],5)
Out[21]:
-6
```
####  isinstance

isinstance(object, classinfo)  判断一个对象是否是一个已知的类型。

```
isinstance(object, class-or-type-or-tuple) -> bool

Return whether an object is an instance of a class or of a subclass thereof.
With a type as second argument, return whether that is the object's type.
The form using a tuple, isinstance(x, (A, B, ...)), is a shortcut for
isinstance(x, A) or isinstance(x, B) or ... (etc.).
```

其第一个参数为对象，第二个为类型名或类型名的一个列表。其返回值为布尔型。若对象的类型与参数二的类型相同则返回True。若参数二为一个元组，则若对象类型与元组中类型名之一相同即返回True。

```
tes = list()
tes.append(3)
ta= 'a','b'
print tes
print isinstance(tes, list)
print isinstance(ta, tuple)

print isinstance(tes,(int, tuple, list))
print isinstance(tes,(int, tuple))
```

Out:

```
    [3]
    True
    True
    True
    False
```

####  id
查看object 地址

in：
`     print id.__doc__
`
 
 Out：​
 `     id(object) -> integer
`
```
    Return the identity of an object.  This is guaranteed to be unique among
    simultaneously existing objects.  (Hint: it's the object's memory address.)
```

####  assert
断言

Python中assert用来判断语句的真假，如果**为假的时候，将触发AssertionError错误**

```
a = None
assert a != None
print a
```

out：


```       ---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-1-4165ca7bb901> in <module>()
      1
      2 a = None
----> 3 assert a != None
      4 print a

AssertionError:
```

另一例子：

```
a=[1,2,3]
# a = None
assert len(a) < 5  # 为真通过
assert a != None
print a
```
out：

```
[1, 2, 3]
```

####  min && max

```
c = [-10, -45, 0, 5, 3, 50, 15, -20, 25]

print "min: %s" %c.index(min(c))  # 返回最小值
print "max: %s" %c.index(max(c)) # 返回最大值
```
        
out:
```
        min: 1
        max: 5
```


