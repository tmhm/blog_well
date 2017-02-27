+++ 
title = "codility lessons" 
date = "Tue, 30 Aug 2016 14:19:00 GMT" 
categories = ["codility"] 
description = "learning in codility. " 
+++ 

```
<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css">

<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>

<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript">
 $(document).ready(function() {
        $("h2,h3,h4,h5,h6").each(function(i,item){
        var tag = $(item).get(0).localName;
        $(item).attr("id","wow"+i);
        $(".newh2").css("margin-left",0);
        $(".newh3").css("margin-left",20);
        $(".newh4").css("margin-left",40);
        $(".newh5").css("margin-left",60);
        $(".newh6").css("margin-left",80);
});
 });
</script>
<div id="category"></div>
```
> --------


[codility lessons](#1)

* [lesson 1: *Iterations*](#1.1)
	* [1. BinaryGap](#1.1.1)
        
* [lesson 2: *Arrays*](#1.2)
    * [1. CyclicRotation](#1.2.1)
    * [2. OddOccurrencesInArray](#1.2.2)

* [lesson 3: *Time complexity*](#1.3)
    * [1. TapeEquilibrium](#1.3.1)
    * [2. FrogJmp](#1.3.2)
    * [3. PermMissingElem](#1.3.3)

* [lesson 4: *counting elements*](#1.4)
    * [1. FrogRiverOne](#1.4.1)
    * [2. PermCheck](#1.4.2)
    * [3. MissingInteger](#1.4.3)
    * [4. MaxCounters](#1.4.4)

* [lesson 5: *prefix sums*](#1.5)
    * [1. PassingCars](#1.5.1)
    * [2. CountDiv](#1.5.2)
    * [3. GenomicRangeQuery](#1.5.3)
    * [4. MinAvgTwoSlice](#1.5.4)

* [lesson 6: *sorting*](#1.6)
    * [1. Distinct](#1.6.1)
    * [2. MaxProductOfThree](#1.6.2)
    * [3. Triangle](#1.6.3)
    * [4. none](#1.6.4)

* [lesson 7: *stacks and queues*](#1.7)
    * [1. none](#1.7.1)
    * [2. Nesting](#1.7.2)
    * [3. none](#1.7.3)
    * [4. MaxCounters](#1.4.4)



<h1 id = "1">
codility lessons
</h1>

- 切记临界点， 比如一个最小，最大的值， len(0), max() 无数据等
- 仔细看好题意， 比如是要求从1 开始的list ，就不需要记最好的值，求长度了，只要记下最大值。

<h2 id = "1.1">
lesson 1：<i>Iterations</i>
</h2>

<h3 id = "1.1.1">
1. BinaryGap-----[100%]
</h3>

> Find longest sequence of zeros in binary representation of an integer.

```
from math import log
def solution(N):
    # write your code in Python 2.7

    if N == 1:
        return 0
    for i in xrange(int(log(N,2))+1):

        tmp = 2**i
        #print tmp
        if tmp == N+1 or tmp == N:
            return 0

    strb = bin(N).split("1")
    #print strb[1:-1]
    if strb[1:-1] is None:
        #print "test"
        return 0
    return max([len(l) for l in strb[1:-1]])
```

<h2 id = "1.2">
lesson 2: <i>Arrays</i>
</h2>

<h3 id = "1.2.1">
1. CyclicRotation------[100%]
</h3>

> Rotate an array to the right by a given number of steps.

```

def re_enumerate(seq):
    n = -1
    length = len(seq)
    for elem in reversed(seq):
        yield length + n, elem
        n = n - 1

def solution(A, K):
    # write your code in Python 2.7
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

<h3 id = "1.2.2">
2. OddOccurrencesInArray ---[100%]
</h3>

> Find value that occurs in odd number of elements.

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

<h2 id = "1.3">
lesson 3: <i>Time complexity</i>
</h2>

<h3 id = "1.3.1">
1. TapeEquilibrium -----[100%]
</h3>

> Minimize the value
|(A[0] + ... + A[P-1]) - (A[P] + ... + A[N-1])|.

```
def solution(A):
    # write your code in Python 2.7

    left,right =A[0], sum(A)-A[0]
    result = abs(right - left)

    for elem in A[1:-1]:
        left,right = left + elem, right - elem
        retmp = abs(right - left)
        if retmp < result:
            result = retmp
    return result
```

<h3 id = "1.3.2">
2. FrogJmp -----[100%]
</h3>


>Count minimal number of jumps from position X to Y.


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

<h3 id = "1.3.3">
3. PermMissingElem -----[100%]
</h3>


>Find the missing element in a given permutation.

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



<h2 id = "1.4">
lesson 4: <i>counting elements</i>
</h2>


<h3 id = "1.4.1">
1. FrogRiverOne----[100%]
</h3>

> Find the earliest time when a frog can jump to the other side of a river.

```
def solution(X, A):
    # write your code in Python 2.7
    ret = -1
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
仔细看好题意， 比如是要求从1 开始的list ，
就不需要记最好的值，求长度了，只要记下最大值。

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







<h3 id = "1.4.3">
3. MissingInteger----[100%]
</h3>


> Find the minimal positive integer not occurring in a given sequence.

**note:**
考虑到不能排序， 返回解一定会在1~len(所给数组)+1的区间。用map记录这些数据是否都在，不在的话，找最小的空缺，都在的时候就是map/list最后一个数加1.

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



<h3 id = "1.4.4">
4. MaxCounters
</h3>


> Calculate the values of counters after applying all alternating operations:
increase counter by 1; set value of all counters to current maximum.


#####  解法一：

- Detected time complexity: O(N*M)

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
分别记录上次update的值，在update的基础上，再记录当前最大值。

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




<h2 id = "1.5">
lesson 5: <i>prefix sums</i>
</h2>


<h3 id = "1.5.1">
1. <i> PassingCars</i>
</h3>

- [100%]
>
Count the number of passing cars on the road.

- Detected time complexity:
O(N)

```
def solution(A):
    # write your code in Python 2.7
    total = sum(A)
    ret = 0
    for elem in A:
        if elem == 0:
            ret += total
            if ret > 1000000000:
                return -1
        else:
            total -= elem
    return ret
```


<h3 id = "1.5.2">
2. <i> CountDiv </i>
</h3>

> Compute number of integers divisible by k in range [a..b].

#####  CountDiv solution 1
- test score 67%

```
def solution(A, B, K):
    # write your code in Python 2.7
    ret = 0
    if A%K == 0:
        ret += 1
    if A != B and B%K == 0 :
        ret += 1

    if (B-A)%K == 0:
        if ret == 2:
            ret += ((B-A)/K - 1)
        else:
            ret += (B-A)/K
    else:
        ret += (B-A)/K
    return ret
```


#####  CountDiv solution 2
- Test score 100%

```
def solution(A, B, K):
    # write your code in Python 2.7
    ret = 0
    ra = -1 if A == 0 else (A - 1)/K
    rb = B/K

    return rb -ra

```
#####  solution 3
- Test score 100%

```
def solution(A, B, K):
    # write your code in Python 2.7
    c = 1 if A%K == 0 else 0
    return B/K -A/K + c

```

<h3 id = "1.5.3">
3. GenomicRangeQuery
</h3>


> Find the minimal nucleotide from a range of sequence DNA.

#####  解法一：
- Test score  37%


```
def getMinFactor(S,l,i,j):
    if j == (l-1):
        tmp = set(S[i:-1])  # error ，should be s[i:]
    else:
        tmp = set(S[i:(j+1)])
    if 'A' in tmp:
        return 1
    elif 'C' in tmp:
        return 2
    elif 'G' in tmp:
        return 3
    else:
        return 4

def solution(S, P, Q):
    # write your code in Python 2.7
    length = len(S)
    result = []
    for x,y in zip(P,Q):
        #print x,y
        result.append(getMinFactor(S,length,x,y))
    return result
```


#####  解法二：
- Test score 100%
> used each list to save that states whether has element or not

```
def calcPrefixSum(s,l):
    category_a, category_c, category_g = [0]*(l+1), [0]*(l+1), [0]*(l+1)
    for i in xrange(l):
        a,c,g = 0,0,0
        if s[i] == 'A':
            a = 1
        elif s[i] == 'C':
            c = 1
        elif s[i] == 'G':
            g = 1
        category_a[i+1] += category_a[i] + a
        category_c[i+1] += category_c[i] + c
        category_g[i+1] += category_g[i] + g

    return category_a, category_c, category_g

def solution(S, P, Q):
    # write your code in Python 2.7

    length = len(S)
    result = []
    a, c, g =  calcPrefixSum(S,length)
    for x,y in zip(P,Q):
        #print x,y
        #result.append(getMinFactor(S,length,x,y))
        if a[y+1] - a[x] > 0:
            result.append(1)
        elif c[y+1] - c[x] > 0:
            result.append(2)
        elif g[y+1] - g[x] > 0:
            result.append(3)
        else:
            result.append(4)

    return result
```


<h3 id = "1.5.4">
4. MinAvgTwoSlice
</h3>


> Find the minimal average of any slice containing at least two elements.

- Test score 100%

**note:** transfer to 2/3


```
def solution(A):
    # write your code in Python 2.7
    length = len(A)
    minStartPos = 0
    minSum = (A[0] + A[1])/2.0

    for i in xrange(length - 2):
        tmp = (A[i] + A[i+1])/2.0
        if tmp < minSum:
            minSum = tmp
            minStartPos = i
        tmp = (tmp*2 + A[i+2])/3.0
        if tmp < minSum:
            minSum = tmp
            minStartPos = i
    if (A[-1] + A[-2])/2.0 < minSum:
        minStartPos = length - 2

    return minStartPos
```

<h2 id = "1.6">
lesson 6: <i>sorting</i>
</h2>


<h3 id = "1.6.1">
1. Distinct
</h3>


> Compute number of distinct values in an array.
- Test score 100%


```
def solution(A):
    # write your code in Python 2.7
    Aset = set(A)
    return len(Aset)
```

<h3 id = "1.6.2">
2. MaxProductOfThree
</h3>


>
Maximize A[P] * A[Q] * A[R] for any triplet (P, Q, R).

#####  solution 1
- O(N)
- Test score  100% OJ test is O(N * log(N))


```
def solution(A):
    ma1, ma2, ma3 = -1000, -1000, -1000
    mi1, mi2 = 1000, 1000
    for elem in A:
        if elem > ma1:
            ma1, ma2, ma3 = elem, ma1, ma2
        elif elem > ma2:
            ma2, ma3 = elem, ma2
        elif elem > ma3:
            ma3 = elem

        if elem < mi1:
            mi1,mi2 = elem, mi1
        elif elem < mi2:
            mi2 = elem
    a, b = ma1*ma2*ma3, ma1*mi1*mi2
    return a if a > b else b
```

#####  solution 2

**note:**

- just need return the value of the max product,
- so, we can just consider the first or last teiplet, after sort

- Detected time complexity: O(N * log(N))

```
def solution(A):
    A.sort()
    return max(A[0]*A[1]*A[-1], A[-1]*A[-2]*A[-3])

```


<h3 id = "1.6.3">
3. Triangle
</h3>


> Determine whether a triangle can be built from a given set of edges.
[https://codesays.com/2014/solution-to-triangle-by-codility/](https://codesays.com/2014/solution-to-triangle-by-codility/)


```
def solution(A):
    # write your code in Python 2.7
    length = len(A)
    if length < 3:
        return 0
    A.sort()
    for idx in xrange(0,length -2):
        if A[idx]+A[idx + 1] > A[idx + 2]:
            return 1
    return 0
```




<h2 id = "1.7">
lesson 7: <i>stacks and queues</i>
</h2>


<h3 id = "1.7.2">
2. Nesting
</h3>


> Determine whether given string of parentheses is properly nested.

- Test score 100%

```
def solution(S):
    # write your code in Python 2.7
    tmp = 0
    for elem in S:
        if elem == "(":
            tmp += 1
        elif elem == ")":
            tmp -= 1
            if tmp < 0:
                return 0
    if tmp == 0:
        return 1
    else:
        return 0
```

<h3 id = "1.7.4">
4. StoneWall
</h3>

> Cover "Manhattan skyline" using the minimum number of rectangles.

- Test score 100%

```
def solution(H):
    # write your code in Python 2.7
    cnt = 0
    stack = []
    for elem in H:
        while len(stack)!= 0 and stack[-1] > elem:
            stack.pop()

        if len(stack) != 0 and stack[-1] == elem:
            pass
        else:
            stack.append(elem)
            cnt += 1
    return cnt

```



