+++ 
title = "prefix sums--codility" 
date = "Sun, 04 Sep 2016 09:08:00 GMT" 
tags = ["codility"] 
categories = ["Algorithm"]
description = "prefix sums练手" 
+++ 

* [1. PassingCars](#1.5.1)

* [2. CountDiv](#1.5.2)
* [3. GenomicRangeQuery](#1.5.3)
* [4. MinAvgTwoSlice](#1.5.4)


<h3 id = "1.5">
lesson 5: <i>prefix sums</i>
</h3>

<h4 id = "1.5.1">
1. <i> PassingCars</i>
</h4>


>
Count the number of passing cars on the road.

A non-empty zero-indexed array A consisting of N integers is given. The consecutive elements of array A represent consecutive cars on a road.

Array A contains only 0s and/or 1s:

0 represents a car traveling east,
1 represents a car traveling west.
The goal is to count passing cars. We say that a pair of cars (P, Q), where 0 ≤ P < Q < N, is passing when P is traveling to the east and Q is traveling to the west.

For example, consider array A such that:

```
  A[0] = 0
  A[1] = 1
  A[2] = 0
  A[3] = 1
  A[4] = 1
```

We have five pairs of passing cars: (0, 1), (0, 3), (0, 4), (2, 3), (2, 4).

**Assume that:**

- N is an integer within the range [1..100,000];
- each element of array A is an integer that can have one of the following values: 0, 1.

**Complexity:**

- expected worst-case time complexity is O(N);
- expected worst-case space complexity is O(1),

思路：

- 可以计算suffix sum的方式
- 然后，从前面开始遍历list，遇到a ＝ 0，result即加上当前的suffix sum的值
- 此题元素是0，1，故可以不用保留每一步的计算，题目有要求限制O(1)的space， 也是给出提示，用一个变量retsum值来记录，每一步的prefix sum值，每移动一步，元素是1的话，将retsum 减1， 即是下一个prefix sum 值。

- Detected time complexity: O(N)
- [100%]

```
def solution(A):
    # write your code in Python 2.7
    result = 0
    retsum = sum(A)
    for a in A:
        if a == 0:
            result += retsum
            if result > 1000000000:
                return -1
        else:
            retsum -= 1
    return result
```



<h4 id = "1.5.2">
2. <i> CountDiv </i>
</h4>

> Compute number of integers divisible by k in range [a..b].


given three integers A, B and K, returns the number of integers within the range [A..B] that are divisible by K, i.e.:

{ i : A ≤ i ≤ B, i mod K = 0 }

For example, for A = 6, B = 11 and K = 2, your function should return 3, because there are three numbers divisible by 2 within the range [6..11], namely 6, 8 and 10.

Assume that:

- A and B are integers within the range [0..2,000,000,000];
- K is an integer within the range [1..2,000,000,000];
A ≤ B.

**Complexity:**

- expected worst-case time complexity is O(1);
- expected worst-case space complexity is O(1).


#####  CountDiv solution 1
- Test score 100%

```
def solution(A, B, K):
    # write your code in Python 2.7
    ra = -1 if A == 0 else (A - 1)/K
    rb = B/K

    return rb -ra

```
#####  solution 2
- Test score 100%

```
def solution(A, B, K):
    # write your code in Python 2.7
    c = 1 if A%K == 0 else 0
    return B/K -A/K + c

```

```
def solution(A, B, K):
    # write your code in Python 2.7
    return (B/K - A/K) if (A%K != 0 ) else (B/K - A/K + 1)
```


<h4 id = "1.5.3">
3. GenomicRangeQuery
</h4>


> Find the minimal nucleotide from a range of sequence DNA.

A DNA sequence can be represented as a string consisting of the letters A, C, G and T, which correspond to the types of successive nucleotides in the sequence. Each nucleotide has an impact factor, which is an integer. Nucleotides of types A, C, G and T have impact factors of 1, 2, 3 and 4, respectively. You are going to answer several queries of the form: What is the minimal impact factor of nucleotides contained in a particular part of the given DNA sequence?

The DNA sequence is given as a non-empty string S = S[0]S[1]...S[N-1] consisting of N characters. There are M queries, which are given in non-empty arrays P and Q, each consisting of M integers. The K-th query (0 ≤ K < M) requires you to find the minimal impact factor of nucleotides contained in the DNA sequence between positions P[K] and Q[K]\(inclusive).

For example, consider string S = CAGCCTA and arrays P, Q such that:

```
    P[0] = 2    Q[0] = 4
    P[1] = 5    Q[1] = 5
    P[2] = 0    Q[2] = 6
```

The answers to these M = 3 queries are as follows:

- The part of the DNA between positions 2 and 4 contains nucleotides G and C (twice), whose impact factors are 3 and 2 respectively, so the answer is 2.
- The part between positions 5 and 5 contains a single nucleotide T, whose impact factor is 4, so the answer is 4.
- The part between positions 0 and 6 (the whole string) contains all nucleotides, in particular nucleotide A whose impact factor is 1, so the answer is 1.

the function should return the values [2, 4, 1], as explained above.

Assume that:

- N is an integer within the range [1..100,000];
- M is an integer within the range [1..50,000];
- each element of arrays P, Q is an integer within the range [0..N − 1];
- P[K] ≤ Q[K], where 0 ≤ K < M;
- string S consists only of upper-case English letters A, C, G, T.

Complexity:

- expected worst-case time complexity is O(N+M);
- expected worst-case space complexity is O(N), beyond input storage (not counting the storage required for input arguments).


#####  解法一：
- Test score  62%
- Detected time complexity:
O(N * M)
- 最初的想法是：根据给出的范围，用set保存，看是否有各元素
- 时间复杂度，明显达不到O(N+M)


```
def getMinFactor(S,l,i,j):
    if j == (l-1):
        tmp = set(S[i:])  #
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
- used each list to save that states whether has element or not
- 在用prefix sum 做差的方式，依次检查是否存在A,C,G,T字符
- 注意数值关系


```
def calcPrefixSum(S):
    l = len(S)+1
    pa,pc,pg = [0]*l,[0]*l,[0]*l
    for idx,elem in enumerate(S):
        a,c,g = 0,0,0
        if elem == 'A':
            a = 1
        elif elem == 'C':
            c = 1
        elif elem == 'G':
            g = 1
        pa[idx+1] = pa[idx] + a
        pc[idx+1] = pc[idx] + c
        pg[idx+1] = pg[idx] + g
    return pa,pc,pg

def solution(S, P, Q):
    # write your code in Python 2.7
    pA,pC,pG = calcPrefixSum(S)
    result = []
    for i,j in zip(P,Q):
        if pA[j+1] - pA[i] > 0:
            ret = 1
        elif pC[j+1] - pC[i] > 0:
            ret = 2
        elif pG[j+1] - pG[i] > 0:
            ret = 3
        else:
            ret = 4
        result.append(ret)
    return result

```

```
根据prefix sum list:
pA = [0, 0, 1, 1, 1, 1, 1, 2]
pC = [0, 1, 1, 1, 2, 3, 3, 3]
pG = [0, 0, 0, 1, 1, 1, 1, 1]

故，是下标［j＋1］－［i］
```


<h4 id = "1.5.4">
4. MinAvgTwoSlice
</h4>


> Find the minimal average of any slice containing at least two elements.

A non-empty zero-indexed array A consisting of N integers is given. A pair of integers (P, Q), such that 0 ≤ P < Q < N, is called a slice of array A (notice that the slice contains at least two elements). The average of a slice (P, Q) is the sum of A[P] + A[P + 1] + ... + A[Q] divided by the length of the slice. To be precise, the average equals (A[P] + A[P + 1] + ... + A[Q]) / (Q − P + 1).

For example, array A such that:

```
    A[0] = 4
    A[1] = 2
    A[2] = 2
    A[3] = 5
    A[4] = 1
    A[5] = 5
    A[6] = 8
```

contains the following example slices:

- slice (1, 2), whose average is (2 + 2) / 2 = 2;
- slice (3, 4), whose average is (5 + 1) / 2 = 3;
- slice (1, 4), whose average is (2 + 2 + 5 + 1) / 4 = 2.5.

The goal is to find the starting position of a slice whose average is minimal.

Complexity:

- expected worst-case time complexity is O(N);
- expected worst-case space complexity is O(N),

#####  sloution
- Test score 100%

**note:** transfer to 2/3 ,
- 只要查看相邻两个和三个的数的平均值即可
- [proof](https://github.com/daotranminh/playground/blob/master/src/codibility/MinAvgTwoSlice/proof.pdf)


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



