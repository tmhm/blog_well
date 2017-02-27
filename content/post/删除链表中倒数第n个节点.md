+++ 
title = "删除链表中倒数第n个节点" 
date = "Fri, 25 Mar 2016 09:25:00 GMT" 
categories = ["算法"] 
description = " 刷题也是提高算法能力的一种方式。" 
+++ 

remove Nth node from end of list</p>
<p><strong>描述：</strong></p>
<p>　　给定一个链表，删除链表中倒数第n个节点，返回链表的头节点。</p>
<div class="m-t-lg m-b-lg"><strong>样例：</strong>
<div class="m-t-sm">
<p>　　删除倒数第二个节点之后，这个链表将变成<strong><span style="color: #e76363;">1-&gt;2-&gt;3-&gt;5-&gt;null.</span></strong></p>
</div>
</div>
<div class="m-t-lg m-b-lg"><strong>挑战：</strong>
<div class="m-t-sm">
<p>　　O(n)时间复杂度</p>
<p><strong>思路：</strong></p>
<p><strong>　　</strong>先求得链表长度，查询链表依次时间O(n)，第二次找到倒数n位置，删除之，时间共O(n)+O(n)~ O(n)</p>
</div>
</div>
<div class="cnblogs_code">
<pre><span style="color: #008080;"> 1</span> <span style="color: #800000;">"""</span>
<span style="color: #008080;"> 2</span> <span style="color: #800000;">Definition of ListNode
</span><span style="color: #008080;"> 3</span> <span style="color: #800000;">class ListNode(object):
</span><span style="color: #008080;"> 4</span>
<span style="color: #008080;"> 5</span> <span style="color: #800000;">    def __init__(self, val, next=None):
</span><span style="color: #008080;"> 6</span> <span style="color: #800000;">        self.val = val
</span><span style="color: #008080;"> 7</span> <span style="color: #800000;">        self.next = next
</span><span style="color: #008080;"> 8</span> <span style="color: #800000;">"""</span>
<span style="color: #008080;"> 9</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> Solution:
</span><span style="color: #008080;">10</span>     <span style="color: #800000;">"""</span>
<span style="color: #008080;">11</span> <span style="color: #800000;">    @param head: The first node of linked list.
</span><span style="color: #008080;">12</span> <span style="color: #800000;">    @param n: An integer.
</span><span style="color: #008080;">13</span> <span style="color: #800000;">    @return: The head of linked list.
</span><span style="color: #008080;">14</span>     <span style="color: #800000;">"""</span>
<span style="color: #008080;">15</span>     <span style="color: #0000ff;">def</span><span style="color: #000000;"> removeNthFromEnd(self, head, n):
</span><span style="color: #008080;">16</span>         <span style="color: #008000;">#</span><span style="color: #008000;"> write your code here</span>
<span style="color: #008080;">17</span>         dummy =<span style="color: #000000;"> ListNode(0)
</span><span style="color: #008080;">18</span>         dummy.next =<span style="color: #000000;"> head
</span><span style="color: #008080;">19</span>
<span style="color: #008080;">20</span>         <span style="color: #008000;">#</span><span style="color: #008000;"> for length</span>
<span style="color: #008080;">21</span>         len =<span style="color: #000000;"> 0
</span><span style="color: #008080;">22</span>         pre =<span style="color: #000000;"> dummy
</span><span style="color: #008080;">23</span>         <span style="color: #0000ff;">while</span><span style="color: #000000;"> pre.next:
</span><span style="color: #008080;">24</span>             len += 1
<span style="color: #008080;">25</span>             pre =<span style="color: #000000;"> pre.next
</span><span style="color: #008080;">26</span>
<span style="color: #008080;">27</span>         <span style="color: #008000;">#</span><span style="color: #008000;"> find the Nth node from end of list</span>
<span style="color: #008080;">28</span>         cnt =<span style="color: #000000;"> 0
</span><span style="color: #008080;">29</span>         tarCnt = len -<span style="color: #000000;"> n
</span><span style="color: #008080;">30</span>         pre =<span style="color: #000000;"> dummy
</span><span style="color: #008080;">31</span>         <span style="color: #0000ff;">while</span><span style="color: #000000;"> pre.next:
</span><span style="color: #008080;">32</span>             <span style="color: #0000ff;">if</span> cnt ==<span style="color: #000000;"> tarCnt:
</span><span style="color: #008080;">33</span>                 pre.next =<span style="color: #000000;"> pre.next.next
</span><span style="color: #008080;">34</span>                 <span style="color: #0000ff;">break</span>
<span style="color: #008080;">35</span>             <span style="color: #0000ff;">else</span><span style="color: #000000;">:
</span><span style="color: #008080;">36</span>                 cnt +=1
<span style="color: #008080;">37</span>                 pre =<span style="color: #000000;"> pre.next
</span><span style="color: #008080;">38</span>
<span style="color: #008080;">39</span>         <span style="color: #0000ff;">return</span> dummy.next</pre>
</div>
<p>&nbsp;</p>



