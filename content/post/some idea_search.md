+++ 
title = "求解路由路径问题（华为挑战赛小思）" 
date = "Mon, 11 Apr 2016 13:47:00 GMT" 
tags = ["Life"] 
categories = ["Algorithm"]
description = " 许久没参加比赛了，一点点记录。" 
+++ 


赛题源自“未来网络”业务发放中的路由计算问题。

######  问题定义
给定一个带权重的有向图G=(V,E)，V为顶点集，E为有向边集，每一条有向边均有一个权重。对于给定的顶点s、t，以及V的子集V'，寻找从s到t的不成环有向路径P，使得P经过V'中所有的顶点(对经过V'中节点的顺序不做要求)。
若不存在这样的有向路径P，则输出无解，程序运行时间越短，则视为结果越优；若存在这样的有向路径P，则输出所得到的路径，路径的权重越小，则视为结果越优，在输出路径权重一样的前提下，程序运行时间越短，则视为结果越优。

######  说明：
- 1）图中所有权重均为[1，20]内的整数；
- 2）任一有向边的起点不等于终点；
- 3）连接顶点A至顶点B的有向边可能超过一条，其权重可能一样，也可能不一样；
- 4）该有向图的顶点不会超过600个，每个顶点出度(以该点为起点的有向边的数量)不超过8；
- 5）V'中元素个数不超过50；
- 6）从s到t的不成环有向路径P是指，P为由一系列有向边组成的从s至t的有向连通路径，且不允许重复经过任一节点；
- 7）路径的权重是指所有组成该路径的所有有向边的权重之和。
######  最终得分机制
华为后台会使用N个测试用例判题，该N个测试用例分为初级、中级、高级三个等级，参赛者对于每个测试用例都会得到一个百分制分数，使用加权平均分(初级权重为0.2，中级权重为0.3，高级权重为0.5)作为该参赛者的最终得分。

输入输出采用文件的形式。

##### 思：
对于上述，这么一个问题，
最初的想法是根据dijkstra最短路径算法中贪心的思想，结合剪枝进行求解，但是对于复杂图节点的情况，主要是在于当必经节点变多，这个规则感觉就太多，没法继续下去。

然后，接触到蚁群算法。
我从旅行商问题（TSP）开始学习这个算法，但是对于我们这个要求经过给定必经点的问题，跟TSP还是有比较大的差距。
TSP问题得以求解的核心思想在于：
*每只蚂蚁经过所有的节点，蚂蚁行走相对于cpu总的频率是一样的，当路径越短，其蚂蚁行走的次数就会增大*

然后，突然有了一个这样类蚂蚁的想法，接下来就是围绕这个想法在实现了。
此处，仅仅是自己的一个思路的回顾，可能会存在一些问题。仅供讨论。
###### 初衷：

**将所有蚂蚁丢在 起点和必经点上，然后再让他们自己去走，走到另一必经点，其信息素增大， 如果走到终点，信息素更大，直到某只蚂蚁找到终点。每次信息素更新时，信息素更新量的大小跟该蚂蚁走过的node数量和是否经过必经点有关。 这样来，理想情况下，每次更新时，经过必经点的路径信息素应该是比较大的，每次更新信息素后，进行一次从起点开始贪心搜，每次走信息素最大的，看是否可以走到终点，如果走到终点，本次路径即是一个解。**

**因为，我们蚂蚁在每个节点，可供选择的节点是很少的， 最多才8个，而tsp问题是可以去任意个节点的 ，这样更新一遍信息素，就可以去类贪心一次，看是否可以找到解。**

######  于是，开始慢慢地折腾和实现了 :-)
####### 类邻接表的图表结构

头结点表

        struct AlGraph{
    EdgeNode *adjacencyNode[8];  //
    AlGraph *algacency_alGraph[8];  //point to the adjacencyNode struct !!!
    short int edgecnt;   // the num of node link to this head node
    short int vertexNum;
    long distToSource;    // distance from start to this node  ,default = 0;
                            // due to the pre cost among [1:20]
    bool known;         // degault = false,  become to true when parse it.
    short int path;    //default = -1  // due to the pre node value among [0:600], yes , are node
    /*这样的缺点是,不能同时找多条路径,一个点只能记录一个它的path,一个node有多个入度的时候,就会出问题了!
        此path成员只在dijkstra里面用，到后面类蚁群算法并没有用，它通过每只蚂蚁走过的路径来体现*/
        };

边节点，它也可能是一个头结点

        struct EdgeNode   //边表节点
        {
           short int adjvex;  //bian jiedian hao
           short int cost;   //cost
           short int linkID;  //the edge id :  point to this node

           /** ant  */
           float pheromone; //
           //short int pheromone_delta;  //改写到蚂蚁身上
           long long nth_antPathVisited;  //nth bits;   all 64  ,实际上只是用来确定该蚂蚁走过这条边, 这样可以尝试走通往该节点的多条边.
           /**
           * 1111 ... ... 1111
           *64,63,... , 3,2,1 nth ant
           **/
        };

必经节点结构

        struct demandIncNode{
            short int demandID;
            bool gotStatus;
        };
        struct demandNode{
            short int source_id;
            short int destination_id;
            bool reachEndFlag;
            short int includingCnt;
            demandIncNode *includingNode[50];
        };

蚂蚁结构

        struct sAnt{
            long int ant_pheromone_Q;   // the gross about pheromone of each ant need to different!
            float pheromone_delta ;

            long l_pathMoved_length ;         //the all length of this ant moved from <their> source!!
            short int CommonNode_movedCount ;         //the number of node had moved
            short int DemandNode_movedCount;    //the number of demand node had moved
            short int cakeOnFront ;

            //float prob[8];    // to my own choose
            float  prob[8];              //the temp for calculate probability to choose
            short int curCandidacyNodeNum; //the number of node can be select to move
            short int passedNodeId;//
            short int curNodeId;  //the node now
            short int nextNodeId; // select node
            short int antStartId;

            bool antDie_status;  // this ant may little value
            bool gotDestination;    //pass some demandNode and got destination.
            bool gotDestFromMidway;
            short int timeCnt;  //let this ant stop ,or they run how much

        };

        struct sAntResult{
            short int toNode;
            short int linkId;
           // sAntResult *next;
           // sAntResult *pre;
            //sAntResult(short int n, short int id) : toNode(n) ,linkId(id) {}
        };
为了方便查找索引，建立了一个头结点索引数组，来存储已经读入头结点的值，来防止访问第一个图表，出现野指针的情况。当需要查找某个头结点的相关信息时，先在头结点数组确认存在后， 再去图\*graph[600]里面直接按头结点id值等于图索引号获取。

###### 算法描述
1. 将图文件中的节点读进来，按照上面的图表格式存储，然后将有出度的头结点用指针联系起来，方便后面的访问。
2. 根据demand节点的个数，初始化蚂蚁的个数，并置位ant相关数据。选择每一只蚂蚁第一次的初始位置（起点+所有必经点+随机的非终点）。
3. 开始进入ant_run的主程序
        1. 将记录蚂蚁经过的路径信息复位；
        2. 让每个蚂蚁run，直到终点，或者没法继续；
                1. 根据当前节点和上一步访问的节点情况，计算本节点出度节点各自的概率，用轮盘选择下一个出度节点（避免一些可推倒的不行解）
                2. 每步记录蚂蚁获得的一些报酬（跟必经点，普通节点，终点，前方有蛋糕的概率等有关），当该蚂蚁停下来时，计算其身上有的信息总量
                3. 待所有蚂蚁都跑完一代，更新他们的信息素到各自走过的路经上
        3. check每只蚂蚁的信息素，并计算各自的增量，叠加到路径的信息素上去；
        4. check本次迭代是否找到解：
                1. 找到一个解（暂时只考虑了一代中多种可行解的优化，和初步环避免）存到一个队列中，返回
                2. 没有找到完整的解，记录返回调试用。目前的路径上的信息素可能有问题，将当次不完整解走过的路径上的信息素，用台风吹吹:-)
        5. 清掉本次迭代图上和ant的各种状态标识等
        6. 直到迭代次数，结束
4. 善后工作，写结果文件，以及调试打印输出等

**上述为程序的主要框架，均按照自己某时想法，一步步累积起来的，更多细节部分细节处理**
相比蚁群算法的核心收敛机制在于cpu的频率，上述方法应该收敛的条件是：

- 经过更多的必经点，蚂蚁报酬更多，从起点达到终点，经过所有必经点，报酬甚之(理想解);
- 可能上一点不完备，有某些问题；
- 结合了强化学习正反馈的报酬机制。


###### 缺陷-反思
缺少完备的理论证明。 并没有证明求解空间在这些规则下，是完备的，并且会收敛到最优(目前来看，收敛性不太好)。
因此，实践起来，发现并不如我心意。
需要在一些规则的限制下才能得到较简单问题的解，目前只能得到case0和case1的解，偶尔会出现次优解。因为，我只在一个地方优化了解*一次迭代中，当出现多个解得时候，即有多只蚂蚁同时获得解，才会去优化次优的信息素*
所以当代与代之间有时不收敛的时候就只能通过其他的规则才能得到解，我一直在想，这个问题，估计我还有某些方面没考虑到，是不完备的解，所以导致不收敛。理论基础好生重要！

软件系统规划仍比较薄弱。 没有学过程序设计模式和架构，都是按照自己的一些想法实现的，当遇到问题的时候，再在这上面添加补充和完善。一开始没有很好的布局式设计，如果一开始能把所有的想明白了，做好整体框架，程序的结构性就会好一些。耦合程度也会低一些。当然在实现的过程中，我也尽量使相关性程度比较高的在一起，为了以后更好地维护。

整个工程的总共时间不长，应该算是我认真做过的项目中最短的了，两个隔离的周末（4天）加基本一周（6天），总共十天（满满的），包括了所有的构思和编码。总共编码在1700行左右，后期并未优化，想想，曾经一个五子棋也写了5000行，当时写dos下的界面花了不少行，当时的AI做的还挺好的，放在班群上也能得到很好的胜率。（当时也实现了自己的一个小想法，不但考虑自己胜算大小，还考虑了对方的，还有梯度的思想，哈哈~）

此处仅当做学习的一个总结，全是自己一行一行码的代码，勿拍砖，不足之处敬请指正与讨论。：-)

其实当每一次认真地做一个project，只要能单纯实现自己的一些想法，也是一件很美妙的事情，目前来看，至少也有6个这样的项目了吧
- 第一块电路板和自制电源
- 独角兽<一代,历时半年多，收货也大大>
- 倒立摆<惊喜之作>
- 四轴<历时也整整半年多，但不太尽人意>
- 五子棋<琢磨>
- 此次也算吧

<感受码代码过程中的----那份喜悦与微妙>

----
独角兽，幸运；更需要实力
2016.4.11晚



