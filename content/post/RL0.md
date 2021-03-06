+++ 
title = "强化学习笔记（0）" 
date = "Thu, 31 Mar 2016 08:18:00 GMT" 
tags = ["RL"] 
categories = ["Machine Learning"]
description = "强化学习入门以及一些资料" 
+++ 


开始接触强化学习是在回到所里开始找课题研究方向的时候，偶然查到[伯克利的机器人视频](http://news.berkeley.edu/2015/05/21/deep-learning-robot-masters-skills-via-trial-and-error/),给我比较惊艳的感觉。陆续开始查找强化学习的一些资料。最近看到一篇有关强化学习的[博文](http://blog.exbot.net/archives/223)，瞬时，想把自己接触到强化学习的过程也做一个简单梳理，为接下来准备在强化学习方向完成毕业论文做一个初略安排。

#####  有关强化学习的资料：
* Tom M. Mitchell的《机器学习》最后一章讲reinforcement learning，算是入门

* [吴恩达](http://www.andrewng.org/)公开课中的强化学习部分，他们[自动直升机实验室](http://heli.stanford.edu/)做的项目。
* 吴恩达的博士生[Pieter Abbeel](http://www.cs.berkeley.edu/~pabbeel/)(现任教于伯克利，上面的机器人即是他们实验室)，他的博士论文Apprenticeship Learning and Reinforcement Learning with Application to Robotic Control值得一看,还有其在伯克利的[DRL课程](http://rll.berkeley.edu/deeprlcourse/)

* [Richard S.Sutton](http://webdocs.cs.ualberta.ca/~sutton/index.html) and [Andrew G.Barto](http://www-anw.cs.umass.edu/~barto/)的RL的经典之作[Reinforcement Learning: An Introduction](http://webdocs.cs.ualberta.ca/~sutton/book/the-book.html)

* Marco Wiering Martijn van Otterlo (Eds.)写的 Reinforcement Learning State-Of-the-Art
* 2015 nature上的 一篇关于 reinforcement learning 的综述: [Reinforcement learning improves behaviour from evaluative feedback（Michael L. Littman）](http://valser.org/thread-246-1-1.html)（链接还包括2015年机器学习专栏其他5篇综述，如下：

       * Machine intelligence  （Tanguy Chouard & Liesbeth Venema）
       * Deep learning（Yann LeCun, Yoshua Bengio & Geoffrey Hinton）
       * Reinforcement learning improves behaviour from evaluative feedback
（Michael L. Littman）
       * Probabilistic machine learning and artificial intelligence（Zoubin Ghahramani）
       * Science, technology and the future of small autonomous drone（Dario Floreano & Robert J. Wood）
       * Design, fabrication and control of soft robots（Daniela Rus & Michael T. Tolley）
       * From evolutionary computation to the evolution of things（Agoston E. Eiben & Jim Smith）

#####  应该关注的实验室：

* [Knowledge-Based Reinforcement Learning](https://www.cs.york.ac.uk/rl/research.php)
* MIT飞行控制实验室[Scaling Reinforcement Learning Methods by Incremental Feature Dependency Discovery](http://acl.mit.edu/projects/iFDD.htm)
* [Reinforcement Learning and Control](http://www.cs.colostate.edu/~anderson/res/rl/)


#####  有关课程:

- [CS 294: Deep Reinforcement Learning](http://rll.berkeley.edu/deeprlcourse/#assignments)
- [Dave Silver’s course on reinforcement learning](http://www0.cs.ucl.ac.uk/staff/D.Silver/web/Teaching.html)


#####  代码工具框架:
* 最近找到一个[PyBrain](http://pybrain.org/)的python开源库，不错,准备前期就用它了

* 看[Pieter Abbeel](http://www.cs.berkeley.edu/~pabbeel/)视频中提到的，他们的库[Computation Graph Toolkit](http://rll.berkeley.edu/cgt/)
* RL-Glue framework

* 网友提到的：
        * ROS的RL
        * 其他：
                * http://www.cse.unsw.edu.au/~cs9417ml/RL1/sourcecode.html
                * http://www.igi.tugraz.at/ril-toolbox/links/
                * http://en.wikipedia.org/wiki/Reinforcement_learning

#####  一些相关会议：
*(from reinforcement learning State of the art)*

A large portion of papers appears every year (or two year) at the established top conferences

* artificial intelligence, such as IJCAI, ECAI and AAAI
* top conferences with a particular focus on statistical machine learning , such as UAI, ICML, ECML and NIPS.
* In addition, conferences on artificial life (Alife), adaptive behavior
(SAB), robotics (ICRA, IROS, RSS) and neural networks and evolutionary computation(e.g. IJCNN and ICANN) feature much reinforcement learning work.

workshop:

* The European workshop on reinforcement learning (EWRL)
* IEEE Symposium on Adaptive Dynamic Programming and Reinforcement Learning (ADPRL)

competitions:

* [RL-Competition](http://www.rl-competition.org/)



