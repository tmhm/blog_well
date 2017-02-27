+++ 
title = "Time complexity--codility" 
date = "Thu, 01 Sep 2016 15:44:00 GMT" 
categories = ["codility"] 
description = "时间复杂度小试" 
+++ 

<h3 id = "1.3">
lesson 3: <i>Time complexity</i>
</h3>

- exercise:
- Problem: You are given an integer n. Count the total of 1+2+...+n.


```
def sumN(N):
    return N*(N+1)//2

```

<h4 id = "1.3.1">
1. TapeEquilibrium -----[100%]
</h4>

> Minimize the value
|(A[0] + ... + A[P-1]) - (A[P] + ... + A[N-1])|.

A non-empty zero-indexed array A consisting of N integers is given. Array A represents numbers on a tape.

Any integer P, such that 0 < P < N, splits this tape into two non-empty parts: A[0], A[1], ..., A[P − 1] and A[P], A[P + 1], ..., A[N − 1].

The difference between the two parts is the value of: |(A[0] + A[1] + ... + A[P − 1]) − (A[P] + A[P + 1] + ... + A[N − 1])|

In other words, it is the absolute difference between the sum of the first part and the sum of the second part.

**note:** 依次求sum

```
def solution(A):

    #left,right =A[0], sum(A)-A[0]
    left,right =A[0], sum(A[1:])
    result = abs(right - left)

    for elem in A[1:-1]:
        left,right = left + elem, right - elem
        retmp = abs(right - left)
        if retmp < result:
            result = retmp
    return result
```

<h4 id = "1.3.2">
2. FrogJmp -----[100%]
</h4>


>Count minimal number of jumps from position X to Y.

A small frog wants to get to the other side of the road. The frog is currently located at position X and wants to get to a position greater than or equal to Y. The small frog always jumps a fixed distance, D.

Count the minimal number of jumps that the small frog must perform to reach its target.


**note:** O(1) time complexity, 注意是否在边界上，否则加1即可。

```
def solution(X, Y, D):
    # write your code in Python 2.7
    if X == Y:
        return 0
    else:
        flag = (Y - X)%D
        ret = (Y - X)/D
        return ret if flag == 0 else ret + 1
```

<h4 id = "1.3.3">
3. PermMissingElem -----[100%]
</h4>


>Find the missing element in a given permutation.

A zero-indexed array A consisting of N different integers is given. The array contains integers in the range [1..(N + 1)], which means that exactly one element is missing.

Your goal is to find that missing element.

**note:**

- 简单思路是排序，然后依此比较是否是增1关系，也可以用求sum的方式
- 注意边界条件，N取值［0，100，000］， 元素取值［1，N+1］，
- 故当没有元素的时候，返回1；当只有一个元素的时候，需要考虑元素是否是1
- 当全部有序的时候，考虑最后元素＋1返回。

```
def solution(A):
    # write your code in Python 2.7
    if len(A) == 0:
        return 1
    elif len(A) == 1:
        return A[0]+1 if A[0] == 1 else A[0] -1
    A.sort()
    left = A[0]
    for elem in A[1:]:
        if elem == left + 1:
            left = elem
            continue
        else:
            return left + 1

    return A[-1]+1 if A[0] == 1 else A[0]-1
```
--------



```
def solution(A):
    # write your code in Python 2.7
    length = len(A)
    if length < 1:
        return 1
    #elif length < 2:  # can belong to the next tatal_sum
    #    return 1 if A[0]==2 else 2

    tatal = sum([i for i in xrange(1,length+2,1) ])
    tmp = sum(A)
    return tatal - tmp

```



