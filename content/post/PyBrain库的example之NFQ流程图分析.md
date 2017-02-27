+++ 
title = "PyBrain库的example之NFQ流程图分析" 
date = "Fri, 27 May 2016 11:43:00 GMT" 
categories = ["Python"] 
description = "PyBrain 库中NFQ算法的流程图分析" 
+++ 


#### PyBrain库的example之NFQ流程图分析

如下是测试程序。主要分析**doEpisode和learn**两个函数。

```
__author__ = 'Thomas Rueckstiess, ruecksti@in.tum.de'

from pybrain.rl.environments.cartpole import CartPoleEnvironment, DiscreteBalanceTask, CartPoleRenderer
from pybrain.rl.agents import LearningAgent
from pybrain.rl.experiments import EpisodicExperiment
from pybrain.rl.learners.valuebased import NFQ, ActionValueNetwork
#,ActionValueLSTMNetwork
from pybrain.rl.explorers import BoltzmannExplorer

from numpy import array, arange, meshgrid, pi, zeros, mean
from matplotlib import pyplot as plt

# switch this to True if you want to see the cart balancing the pole (slower)
render = False  #True #

plt.ion()

env = CartPoleEnvironment()
if render:
    renderer = CartPoleRenderer()
    env.setRenderer(renderer)
    renderer.start()


# balancetask. py inside only used 2 sensors, so here can't use(4,3), just use (2,3)
# there is a debug in vesion 0.30, now, new version 0.33 had correct it!!
module = ActionValueNetwork(4,3)  #(4,3) #  0.33 had correct it
#module = ActionValueLSTMNetwork(2,3)

task = DiscreteBalanceTask(env, 100)
learner = NFQ()
learner.explorer.epsilon = 0.4

agent = LearningAgent(module, learner)
testagent = LearningAgent(module, None)
experiment = EpisodicExperiment(task, agent)


def plotPerformance(values, fig):
    plt.figure(fig.number)
    plt.clf()
    plt.plot(values, 'o-')
    plt.gcf().canvas.draw()


performance = []

if not render:
    pf_fig = plt.figure()

#while (True):
for _ in xrange(60): #60
    # one learning step after one episode of world-interaction!!!
    experiment.doEpisodes(1)
    agent.learn(2)  # 5

    # test performance (these real-world experiences are not used for training)
    if render:
        env.delay = True
    experiment.agent = testagent
    #r = mean([sum(x) for x in experiment.doEpisodes(5)])
    env.delay = False
    testagent.reset()
    experiment.agent = agent

    #performance.append(r)
    print "update step", len(performance)

    #print "reward avg", r
    print "explorer epsilon", learner.explorer.epsilon
    print "num episodes", agent.history.getNumSequences()
    print "update step", len(performance)

if not render:
    plotPerformance(performance, pf_fig)

str = raw_input("please input sth to end!")
print "you put :",str

```

experiment.doEpisodes(1)
![](http://i.imgur.com/Un1ow9B.png)


agent.learn(2)
![](http://i.imgur.com/TQ2YO9y.png)

图2的注释2部分，可以参考该博文[深度强化学习初探](http://lamda.nju.edu.cn/yangjw/project/drlintro.html) ,但是他文中的公式应该有点问题。应该把Qm+1改为Qm，进一步参考维基百科[Q-learning](https://en.wikipedia.org/wiki/Q-learning) ,如下所示。

Qm+1(st,at)=Qm(st,at)+α[rt+1+γQm(st+1,at+1)−Qm(st,at)]



推荐所用的画图软件[process on](https://www.processon.com/i/56873407e4b0b5afb309e00c)

- 用起来挺方便的，在线用谷歌浏览器运行，用户体验挺佳，比visio2010快多了；
- 可以多用户协作；
- 目前有一个缺点就是一个框里面的字体格式必须是一样的，不可以修改一个框里面部分的文字的格式。有点类似PS的思想。



