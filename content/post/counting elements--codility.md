+++ 
title = "counting elements--codility" 
date = "Thu, 01 Sep 2016 15:46:00 GMT" 
tags = ["codility"] 
categories = ["Algorithm"]
description = "计数方法" 
+++ 

* [1. FrogRiverOne](#1.4.1)

* [2. PermCheck](#1.4.2)
* [3. MissingInteger](#1.4.3)
* [4. MaxCounters](#1.4.4)


<h2 id = "1.4">
lesson 4: <i>counting elements</i>
</h2>

- exercise
- Problem: You are given an integer m (1 <= m <= 1,000,000) and two non-empty, zero-indexed arrays A and B of n integers, a0,a1,...,an−1 and b0,b1,...,bn−1 respectively (0 <= ai,bi <= m).

>For every element of array B, we assume that we will swap it with some element from array A. The difference d tells us the value from array A that we are interested in swapping, because only one value will cause the two totals to be equal. The occurrence of this value can be found in constant time from the array used for counting.

**NOTE:**

- 假设A中ai和B中的bj交换可以满足要求，则有：A＋bj－ai ＝＝ B＋ai－bj
- 假设 d ＝ bj － ai， 则有 A ＋ d ＝ B － d
- 故有A－B ＝ 2d， 因此，两个数组的差必须是2的整数倍，即偶数，否则不存在交换可以满足题意的元素
- 然后由d ＝ bj － ai，得到在A中存在元素ai ＝ bj － d。我们可以对每一个B中的元素bj，查找A中是否有bj－d 即可。

```
def counting(A, m):
        n = len(A)
        count=[0]*(m+1)
        for k in xrange(n):
                count[A[k]] += 1
        return count


## from collections import Counter
def fast_solution(A, B, m):
        n = len(A)
        sum_a = sum(A)
        sum_b = sum(B)
        d = sum_b - sum_a
        if d%2==1:
                return False
        d//=2
        count = counting(A, m)
        for i in xrange(n):
                if 0 <= B[i] - d <= m and count[B[i] - d] > 0:
                        return True
        return False

```
> 根本目的是找到A中存在元素ai ＝ bj － d， 前面的Bj －d 范围在 ［0，m］只是限定count的范围，防止越界！
>
> 若使用collections 的Counter 函数计数，则可以直接查看是否存在bj－d这个元素即可，即count[bj-d] > 0要求。
>
> 我们也可以直接用for everyone B element， 判断if （bj － d） in A 也可以,但是这样的复杂度又变高了， 变成O(N^2)了。因为，每次查找（bj － d） 是否in A 的时候就要N次遍历查找。
> **因此，还是用计数的方式好，首先统计A中的元素，以后只要O(1)的时间查找是否存在我们需要的ai。**

可以有如下解答：

**so,we can do it as follows:**

```
from collections import Counter
def fast_solution(A, B, m):
        n = len(A)
        sum_a = sum(A)
        sum_b = sum(B)
        d = sum_b - sum_a
        if d%2==1:
                return False
        d//=2
        count = Counter(A, m)
        for i in xrange(n):
                if count[B[i] - d] > 0:
                        return True
        return False

```



<h3 id = "1.4.1">
1. FrogRiverOne----[100%]
</h3>

> Find the earliest time when a frog can jump to the other side of a river.
>
> The goal is to find the earliest time when the frog can jump to the other side of the river. The frog can cross only when leaves appear at every position across the river from 1 to X (that is, we want to find the earliest moment when all the positions from 1 to X are covered by leaves). You may assume that the speed of the current in the river is negligibly small, i.e. the leaves do not change their positions once they fall in the river.
>
> **each element of array A is an integer within the range [1..X].**

- 鉴于元素的范围，所以，可以用list保存出现过的元素，比较list的长度跟X的关系，就可以得到第一次出现X的位置
- 这里需要考虑的一个问题是，是第一次访问过所有的元素种类，停留的位置不要求必须是X元素；另一种情况是，不仅仅要求访问过所有的元素种类，还要最后停留在X元素上，再算达到要求。**本题解法是第一种情况。**
- 若是第二种情况的话，只需要在set的长度等于X后，多加一次判断，找到接下来第一次出现X的位置即好。

- 鉴于O(N) -time  O(X)-space ,每次在保存过的元素中查找费时间，可以直接用set保存，因为set加入新元素的时候，重复元素不会再次加入，这里应该看看[python源码set](http://svn.python.org/view/python/trunk/Objects/setobject.c?view=markup)怎么加入的。

```
def solution(X, A):
    # write your code in Python 2.7
    visited = set()
    for idx,elem in enumerate(A):
        visited.add(elem)
        if len(visited) == X:
            return idx
    return -1
```





<h3 id = "1.4.2">
2. PermCheck----[100%]
</h3>


> Check whether array A is a permutation.

**note:**

思路一：

- 用最大值是否等于set的长度来测试.
仔细看好题意， 比如是要求从1 开始的list ，
就不需要记最小的值，求长度了，只要记下最大值。

```
def solution(A):
    # write your code in Python 2.7
    #A.sort() # Nlog(N)
    if len(A) == 1:
        return 1 if (A[0] == 1) else 0

    sigle = set()
    #maxelem, minelem = A[0], A[0]   #needless minelem,owing to from 1 !!
    maxelem = A[0]
    for elem in A:
        if elem not in sigle:
            sigle.add(elem)
            if elem > maxelem:
                maxelem = elem
        else:
            #print "test"
            return 0
    #print sigle
    return 1 if (len(sigle) == maxelem) else 0

    #return 1 if (A[-1]-A[0]+1) == len(A) else 0
```

思路二：

- 如果元素的取值是在N个范围内，即数组有N个元素，每个元素范围是[1,N] ,则我们可以用求sum的方式来做 ［score ：100%］


```
def solution(A):
    # write your code in Python 2.7
    set_a = set(A)

    length = len(set_a)
    total = length*(length+1)//2
    tmp = sum(A)
    return 1 if total == tmp else 0

```


<h3 id = "1.4.3">
3. MissingInteger----[100%]
</h3>


> Find the minimal positive integer not occurring in a given sequence.

**note:**

- O(N) time & space complexity,if sort, will exceed.
- 考虑到不能排序，从解空间出发考虑。返回解一定会在1~len(所给数组)+1的区间。用map记录这些数据是否都在，不在的话，找最小的空缺，都在的时候就是map/list最后一个数加1.

```
def solution(A):
    # write your code in Python 2.7
    length = len(A)
    tmp = [True]*(length + 1)
    #print tmp
    for elem in A:
        if 0 < elem < length+1:
            tmp[elem-1] = False
    for idx,elem in enumerate(tmp):
        if elem == True:
            return idx+1
    return length +1
```

```
def solution(A):
    # write your code in Python 2.7
    length = len(A)+1
    tmp = [True]*length
    for a in A:
        if 0 < a < length:
            tmp[a-1] = a
    for idx, a in enumerate(tmp):
        if a is True:
            return idx+1
    #return length

```
*将tmp数组增大一位，就不需要最后考虑全部是有序的，再return length ＋ 1了*

<h3 id = "1.4.4">
4. MaxCounters
</h3>


> Calculate the values of counters after applying all alternating operations:increase counter by 1; set value of all counters to current maximum.


You are given N counters, initially set to 0, and you have two possible operations on them:

- increase(X) − counter X is increased by 1,
- max counter − all counters are set to the maximum value of any counter.

A non-empty zero-indexed array A of M integers is given. This array represents consecutive operations:

- if A[K] = X, such that 1 ≤ X ≤ N, then operation K is increase(X),
- if A[K] = N + 1 then operation K is max counter.

For example, given integer N = 5 and array A such that:

    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4
the values of the counters after each consecutive operation will be:

    (0, 0, 1, 0, 0)
    (0, 0, 1, 1, 0)
    (0, 0, 1, 2, 0)
    (2, 2, 2, 2, 2)
    (3, 2, 2, 2, 2)
    (3, 2, 2, 3, 2)
    (3, 2, 2, 4, 2)
The goal is to calculate the value of every counter after all operations.


#####  解法一：

- Detected time complexity: O(N*M)
- Test score 66%
- Correctness 100%      Performance     60%
- 计算正确，但是时间复杂度不满足要求。

```
def solution(N, A):
    # write your code in Python 2.7
    ret = [0]*N
    #print ret

    for elem in A:
        if elem < (N + 1):
            #print elem
            #print ret
            ret[int(elem -1)] += 1
        else:
            #tmp = max(ret)
            #print "tmp",tmp
            #ret = [tmp]*N
            ret = [max(ret)]*N
            #print "ret",ret
    return ret
```



#####  解法二
[100%] MaxCounters
>
Calculate the values of counters after applying all alternating operations: increase counter by 1; set value of all counters to current maximum.


**note:**

 - 分别记录上次update的值，在update的基础上，再记录当前最大值。
 - 有点类似操作系统的内存管理，按需分配的味道， 当出现缺页中断的时候再去分配内存。
 - 这里是当出现N+1的时候，再去update 上次没有update的元素，并且在最后一次需要再检查一次update


- Detected time complexity:
O(N + M)

```
def solution(N, A):
    # write your code in Python 2.7
    ret = [0]*N

    maxOfArray = 0
    last_update = 0
    tn = N + 1
    for elem in A:
        if elem < tn:
            if ret[elem -1] < last_update:
                ret[elem -1] = last_update

            ret[elem -1] += 1
            if ret[elem -1] > maxOfArray:
                maxOfArray = ret[elem -1]
        else:
            last_update = maxOfArray
            #print "last_update",last_update
            #ret = [maxOfArray]*N

            #print "ret",ret
    for elem in xrange(N):
        if ret[elem] < last_update:
            ret[elem] = last_update
    return ret
```

```
def solution(N, A):
    # write your code in Python 2.7
    tn = N + 1
    maxOfCounter = 0
    lastUpdate = 0
    ret = [0]*N
    for a in A:
        if a < tn:
            if ret[a-1] < lastUpdate :
                ret[a-1] = lastUpdate
            ret[a-1] += 1
            if ret[a-1] > maxOfCounter:
                # update max counter
                maxOfCounter = ret[a-1]
        else:
            lastUpdate = maxOfCounter
    for elem in xrange(N):
        if ret[elem-1] < lastUpdate:
            ret[elem-1] = lastUpdate
    return ret
```



