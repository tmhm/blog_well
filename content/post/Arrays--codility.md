+++ 
title = "Arrays--codility" 
date = "Thu, 01 Sep 2016 15:40:00 GMT" 
tags = ["codility"] 
categories = ["Algorithm"]
description = "数组 " 
+++ 

<h2 id = "1.2">
lesson 2: <i>Arrays</i>
</h2>

- exercise:
- Problem: Given array A consisting of N integers, return the reversed array.


```
def myreverse(A):
    length = len(A)
    for i in xrange(length//2):
        A[i], A[length -i-1] = A[length - i-1], A[i]
    return A

```

<h3 id = "1.2.1">
1. CyclicRotation------[100%]
</h3>

> Rotate an array to the right by a given number of steps.


A zero-indexed array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is also moved to the first place.

For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7]. The goal is to rotate array A K times; that is, each element of A will be shifted to the right by K indexes.

```
def re_enumerate(seq):
    n = -1
    length = len(seq)
    for elem in reversed(seq):
        yield length + n, elem
        n = n - 1

def solution(A, K):
    length = len(A)
    if length < 2:
        return A
    shift_times = K%length
    #print shift_times
    tmp= A[-shift_times:]
    #print tmp
    tmp.extend(A[:- shift_times])
    return tmp
    #for idx, ai in re_enumerate(A):
    #    if idx >= shift_times:
    #        A[idx] = A[idx-shift_times]
    #
    #    else:
    #        A[idx] = tmp[idx]
    #return A

```

- 要注意边界条件，考虑数组的长度少于旋转的次数，以及只有一个元素和数组为空
- 当有除法的时候，注意是否除以零

```
def solution(A, K):
    # write your code in Python 2.7
    length = len(A)
    #print length
    #print K
    if K == 0 or length == 0 or length == 1:
        return A
    if K > length:
        K %= length

    #print K
    tmp = [0]*K
    #print tmp
    for i in xrange(K):
        tmp[i] = A[length - K + i]
    tmp.extend(A[:-K])
    return tmp
```

<h3 id = "1.2.2">
2. OddOccurrencesInArray ---[100%]
</h3>

> Find value that occurs in odd number of elements.

A non-empty zero-indexed array A consisting of N integers is given. The array contains an odd number of elements, and each element of the array can be paired with another element that has the same value, except for one element that is left unpaired.

For example, in array A such that:

```
      A[0] = 9  A[1] = 3  A[2] = 9
      A[3] = 3  A[4] = 9  A[5] = 7
      A[6] = 9
```
- the elements at indexes 0 and 2 have value 9,
- the elements at indexes 1 and 3 have value 3,
- the elements at indexes 4 and 6 have value 9,
- the element at index 5 has value 7 and is unpaired.


解题思路：

- 将数组排序，相同的数会在一起，比较前后两个数是否相同
- 偶数个数的，继续测试下一个数，找到单独的一个数，返回
- 当直到数组最后，还有没有找到单独的一个元素，我们得返回最后一个元素，因为我们统计的一直的是last 元素
- 注意临界条件，只有一个元素
- O(N)时间复杂度

```
def solution(A):
    # write your code in Python 2.7
    if len(A) == 1:
        return A[0]
    A.sort()
    #print A
    last_a,cnt = A[0],1
    #print last_a
    for ai in A[1:]:
        if ai == last_a:
            cnt += 1
        else:
            if cnt%2 != 0:
                return last_a
            last_a,cnt = ai,1

    return A[-1]

```
---------

```
def solution(A):
    # write your code in Python 2.7
    if len(A) < 2:
        return A[0]
    A.sort()
    last_a, cnt = A[0], 1
    for a in A[1:]:
        if last_a == a:
            cnt += 1
        else:
            if cnt%2 == 0 :
                last_a, cnt = a, 1
            else:
                return last_a
    return A[-1]

```



