+++ 
title = "stacks and queues--codility" 
date = "Sat, 17 Sep 2016 10:44:00 GMT" 
categories = ["codility"] 
description = " 队列和栈" 
+++ 

* [1. Nesting](#1.7.1)

* [2. StoneWall](#1.7.2)
* [3. Brackets](#1.7.3)
* [4. Finsh](#1.7.4)


<h3 id = "1.7">
lesson 7: <i>stacks and queues</i>
</h3>


<h4 id = "1.7.1">
1. Nesting
</h4>


> Determine whether given string of parentheses is properly nested.

A string S consisting of N characters is called properly nested if:

- S is empty;
- S has the form "(U)" where U is a properly nested string;
- S has the form "VW" where V and W are properly nested strings.

For example, string "(()(())())" is properly nested but string "())" isn't.

**Assume that:**

- N is an integer within the range [0..1,000,000];
- string S consists only of the characters "(" and/or ")".

**Complexity:**

- expected worst-case time complexity is O(N);
- expected worst-case space complexity is O(1) (not counting the storage required for input arguments).

**solution:**

- Test score 100%
- used stack
- must have "(" before ")"

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






<h4 id = "1.7.2">
2. StoneWall
</h4>

You are going to build a stone wall. The wall should be straight and N meters long, and its thickness should be constant; however, it should have different heights in different places. The height of the wall is specified by a zero-indexed array H of N positive integers. H[I] is the height of the wall from I to I+1 meters to the right of its left end. In particular, H[0] is the height of the wall's left end and H[N−1] is the height of the wall's right end.

The wall should be built of cuboid stone blocks (that is, all sides of such blocks are rectangular). Your task is to compute the minimum number of blocks needed to build the wall.

For example, given array H containing N = 9 integers:

          H[0] = 8    H[1] = 8    H[2] = 5
          H[3] = 7    H[4] = 9    H[5] = 8
          H[6] = 7    H[7] = 4    H[8] = 8

the function should return 7. The figure shows one possible arrangement of seven blocks.

![](https://codility-frontend-prod.s3.amazonaws.com/media/task_img/stone_wall/media/auto/mp2e167f4181a5967a0844fbd70a3a5bfb.png)


**Assume that:**

- N is an integer within the range [1..100,000];
- each element of array H is an integer within the range [1..1,000,000,000].



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

<h4 id = "1.7.3">
3. Brackets

</h4>

> Determine whether a given string of parentheses is properly nested.


**Task description**

A string S consisting of N characters is considered to be properly nested if any of the following conditions is true:

- S is empty;
- S has the form "(U)" or "[U]" or " {
        U
}" where U is a properly nested string;
- S has the form "VW" where V and W are properly nested strings.

For example, the string " {
        [()()]
}" is properly nested but "([)()]" is not.


For example, given S = "{[()()]}", the function should return 1 and given S = "([)()]", the function should return 0, as explained above.

**Assume that:**

- N is an integer within the range [0..200,000];
- string S consists only of the following characters: "(", "{", "[", "]", "}" and/or ")".

**Complexity:**

- expected worst-case time complexity is O(N);
- expected worst-case space complexity is O(N) (not counting the storage required for input arguments).

用一个stack，当栈头和新来的元素配对，即弹出，否则压栈。
注意：list为空的时候， 还有多个空值的list。

**solution：**

```
def check(t,s):
    if len(t) < 1:
        return 0
    if s == ')' and t[-1] == '(':
        return 1
    elif s == ']' and t[-1] == '[':
        return 1
    elif s == '}' and t[-1] == '{':
        return 1
    else:
        return 0


def solution(S):
    tmp = []
    for elem in S:
        if elem == ' ':
            continue
        if check(tmp, elem):
            tmp.pop()
        else:
            tmp.append(elem)
            #print "append: %s, len: %s" %(elem,len(tmp))

    if len(tmp) < 1:
        return 1
    else:
        return 0
```







<h4 id = "1.7.4">

4. Fish

</h4>

Given two non-empty zero-indexed arrays A and B consisting of N integers. Arrays A and B represent N voracious fish in a river, ordered downstream along the flow of the river.

The fish are numbered from 0 to N ? 1. If P and Q are two fish and P < Q, then fish P is initially upstream of fish Q. Initially, each fish has a unique position.

Fish number P is represented by A[P] and B[P]. Array A contains the sizes of the fish. All its elements are unique. Array B contains the directions of the fish. It contains only 0s and/or 1s, where:

- 0 represents a fish flowing upstream,
- 1 represents a fish flowing downstream.

If two fish move in opposite directions and there are no other (living) fish between them, they will eventually meet each other. Then only one fish can stay alive ? the larger fish eats the smaller one. More precisely, we say that two fish P and Q meet each other when P < Q, B[P] = 1 and B[Q] = 0, and there are no living fish between them. After they meet:

- If A[P] > A[Q] then P eats Q, and P will still be flowing downstream,
- If A[Q] > A[P] then Q eats P, and Q will still be flowing upstream.

We assume that all the fish are flowing at the same speed. That is, fish moving in the same direction never meet. The goal is to calculate the number of fish that will stay alive.

For example, consider arrays A and B such that:

          A[0] = 4    B[0] = 0
          A[1] = 3    B[1] = 1
          A[2] = 2    B[2] = 0
          A[3] = 1    B[3] = 0
          A[4] = 5    B[4] = 0

Initially all the fish are alive and all except fish number 1 are moving upstream. Fish number 1 meets fish number 2 and eats it, then it meets fish number 3 and eats it too. Finally, it meets fish number 4 and is eaten by it. The remaining two fish, number 0 and 4, never meet and therefore stay alive.


For example, given the arrays shown above, the function should return 2, as explained above.

**Assume that:**

- N is an integer within the range [1..100,000];
- each element of array A is an integer within the range [0..1,000,000,000];
- each element of array B is an integer that can have one of the following values: 0, 1;
- the elements of A are all distinct.

**Complexity:**

- expected worst-case time complexity is O(N);
- expected worst-case space complexity is O(N), beyond input storage (not counting the storage required for input arguments).


考虑到所有鱼的速度一致，那么从上游开始check，
前面的鱼如果是往上游走的话，即永远不会被吃或者吃其他鱼,

```
def solution(A, B):
    # write your code in Python 2.7
    # record the num of fish with downstream
    lastFishDir = 0
    stackTmp = []

    # check fish from upstream
    for fish, curDir in zip(A,B):
        if lastFishDir < 1:
            stackTmp.append(fish)
            #lastFishDir += curDir
        else:
            if curDir == 0:
                while lastFishDir > 0 and fish > stackTmp[-1]:
                    stackTmp.pop()
                    lastFishDir -= 1
                if len(stackTmp) > 0 and fish < stackTmp[-1]:
                    continue
                stackTmp.append(fish)
            else:
                stackTmp.append(fish)

        lastFishDir += curDir
    return len(stackTmp)
```


思考方式很重要：

由于，上游的鱼如果是往上游走的话，即永远不会被吃或者吃其他鱼,
如果把这样的鱼也放在stack里面，每次fight之后，不太好处理，
故我们可以把一定可以存活的鱼直接计数， 将需要fight的鱼放在stack里面

- [100%]

```
def solution(A, B):
    lastFishDir = 0  # record the num of fish with downstream
    stackDown = []
    aliveCnt = 0

    # check fish from upstream
    for fish, curDir in zip(A,B):
        if curDir == 1:
            # only the downstream fish need fight,
            stackDown.append(fish)
        else:
            while lastFishDir > 0 :
                if fish > stackDown[-1]:
                    stackDown.pop()
                    lastFishDir -= 1
                else:
                    break
            else:
                aliveCnt += 1

        lastFishDir += curDir

    return len(stackDown)+aliveCnt
```

该博主分析的很详细，[https://codesays.com/2014/solution-to-fish-by-codility/](https://codesays.com/2014/solution-to-fish-by-codility/)
>
```
def solution(A, B):
    alive_count = 0        # The number of fish that will stay alive
    downstream = []        # To record the fishs flowing downstream
    downstream_count = 0   # To record the number of elements in downstream

    for index in xrange(len(A)):
        # Compute for each fish
        if B[index] == 1:
            # This fish is flowing downstream. It would
            # NEVER meet the previous fishs. But possibly
            # it has to fight with the downstream fishs.
            downstream.append(A[index])
            downstream_count += 1
        else:
            # This fish is flowing upstream. It would either
            #    eat ALL the previous downstream-flow fishs,
            #    and stay alive.
            # OR
            #    be eaten by ONE of the previous downstream-
            #    flow fishs, which is bigger, and died.
            while downstream_count != 0:
                # It has to fight with each previous living
                # fish, with nearest first.
                if downstream[-1] < A[index]:
                    # Win and to continue the next fight
                    downstream_count -= 1
                    downstream.pop()
                else:
                    # Lose and die
                    break
            else:
                # This upstream-flow fish eat all the previous
                # downstream-flow fishs. Win and stay alive.
                alive_count += 1

    # Currently, all the downstream-flow fishs in stack
    # downstream will not meet with any fish. They will
    # stay alive.
    alive_count += len(downstream)

    return alive_count
```



