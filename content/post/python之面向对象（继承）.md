+++ 
title = "python之面向对象（继承）" 
date = "Thu, 26 May 2016 09:53:00 GMT" 
tags = ["Python"] 
categories = ["Language"]
description = "python之继承思想" 
+++ 


####  python之面向对象（继承）

面向对象的编程带来的主要好处之一是代码的重用，实现这种重用的方法之一是通过继承机制。继承完全可以理解成类之间的类型和子类型关系。


需要注意的地方：继承语法 class 派生类名（基类名）：//... 基类名写作括号里，基本类是在类定义的时候，在元组之中指明的。

在python中继承中的一些特点：

1. 在继承中**基类的构造（__init__()方法）不会被自动调用**，它需要在其派生类的构造中亲自专门调用。

2. 在调用基类的方法时，需要加上基类的类名前缀，且需要带上self参数变量。区别于在类中调用普通函数时并不需要带上self参数.

3. Python总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中**逐个查找**。（先在本类中查找调用的方法，找不到才去基类中找）。

----
```
class animal(object):
    def __init__(self):
        print 'animal init'

    def say(self):
        print 'animal say'

class dog(animal):
    def __init__(self):
        #animal.__init__(self)
        print 'dog init'

    def say(self):
        animal.say(self)
        print 'dog say'
```

In [23]:

```
d = dog()
d.say()
```

out:

```
dog init
animal say
dog say
```

如果在继承元组中列了一个以上的类，那么它就被称作"多重继承" 。
当多个类之间有包含关系，不能继承。

比如，下面的例子不行。

```
        class A:
        class B(A):
        class C(A,B)
```

下面是，多重继承逐个查找的示例：

```
class animal(object):
    def __init__(self):
        print 'animal init'

    def say(self):
        print 'animal say'

class dog(animal):
    def __init__(self):
        #animal.__init__(self)
        print 'dog init'

    def say(self):
        animal.say(self)
        print 'dog say'

class cat(animal):
    def __init__(self):
        #animal.__init__(self)
        print 'cat init'

    def say(self):
        #animal.say(self)
        print 'cat say'

class wolfhound(cat,dog):  #先继承cat，后面测试案例只输出了cat say。
    def __init__(self):
        print "wolfhound init."

    def run(self):
        print "wolfhound run"
```

in：

```
d = dog()
d.say()
w = wolfhound()
w.run()
w.say()
```

out：

```
dog init
animal say
dog say
wolfhound init.
wolfhound run
cat say

```

