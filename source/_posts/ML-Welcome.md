---
title: 机器学习-Welcome
date: 2018-05-23 19:46:14
categories: [Math,Machine Learning]
tags: [Machine Learning,Andrew Ng]
---



当你在网易云点开个性推荐里的`私人FM`和`每日推荐`的时候，就已经在使用`机器学习`带来的成果了。

为什么网易云会知道你的音乐品味？网易云是不是也有“失算”的时候呢？

<!-- more -->

---

![NetEase](/images/MachineLearning-Welcome/WechatIMG49.jpeg)



### 如何定义机器学习？

定义这种事因人而异，与其去接受别人给出的生涩的定义，不如自己去体会、理解。

- 目前比较容易被人接受的两种定义

  - Arthur Samuel: Field of study that gives computers the abilities to learn without being explicitly programmed.

    简单说就是——__授之以鱼，不如授之以渔__。不是给定特定的编程让计算机做特定的事，而是给计算机自己去学习、去做事的能力。

    Samuel当初设计了一个西洋棋的程序，Samuel自己并不是个下期高手，但是——

    ![img](/images/MachineLearning-Welcome/Arthur-1.jpg)![img](/images/MachineLearning-Welcome/Arthur-2.jpg)![img](/images/MachineLearning-Welcome/Arthur-3.jpg)![img](/images/MachineLearning-Welcome/Arthur-4.jpg)![img](/images/MachineLearning-Welcome/Arthur-5.jpg)![img](/images/MachineLearning-Welcome/Arthur-6.jpg)

  - Tom Mitchell: A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E.

    这个说法就比较抽象了，重点需要理解`E`、`T`、`P`三个字母的意思，举个例子：

    比如说训练下棋这个程序：

    `E`：`Experience`，从若干次下棋中获得的经验，就像：什么布局能赢，什么布局容易输等等

    `T`：`Task`，和人下棋就是这个程序的任务

    `P`：`Performance`，赢得棋局的胜率，可以衡量这个程序是否合格

    总结起来，就是：__这个程序能掌握经验，并在经验中能提高执行某些任务成功的几率。__

    很显然 Samuel 的下棋程序做到了，它（程序）已经是个西洋棋高手了！

- 学习是个循序渐进的过程，`机器学习(Machiene Learning)`也是这样。我们学习机器学习的目的不是让自己会`机器学习`，而是让`机器(Machine:Computer)`会`学习(Learn)`。所以，推物及人，人类是如何学习的呢？只有首先理解`人类是如何学习(Learn)的？`这个问题，才能把`机器学习`的问题理解得更好。

  我们举个简单的例子：

  从最初的，学习阿拉伯数字`（0~9）`开始，我们通过一遍一遍反复地去描摹`（0~9）`这十个数字的外形，在脑海记下每一个数字的各种轮廓样本，然后当老师在黑板上写下`"5"`的时候，我们开始在脑海搜索与这个数字外形极其相似的数字样本，当匹配到数字`5`的时候，我们就可以大声地告诉老师：这是数字`5`！

  ---

  这就是在`学习`。有趣的是，`机器学习`在__手写识别__方面使用的`KNN(K Nearest Neighbor Algorithem)`算法（又叫“K近邻算法”），正是使用的上述例子中的`学习过程(Learning Process)`的方法（这个算法以后讨论）。



### 给自己一个规划

其实好多人都已经搭好了自己的个人博客了，还不知道的，可以移步[WIKI](http://wiki.dev.nongfenqi.com/%E8%B5%84%E6%BA%90:%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%BA%90)(内部资源)。

首先，我想说的是——嗯~`Hexo`+`Next`很棒！

`Next`最新版的功能真的是强大，只要你想得到的，几乎都能通过简单的配置得以实现。

有了博客平台，然后就是利用笔记，每天、抑或是每周做点总结，当然，能做出来分享那就更好不过了。

还有一点很重要的就是——做笔记主要目的不是给别人看，就算阅读量每天毫无变化又怎么样？！拿出点上学时代做笔记的精神来，笔记做得好，才能学得好！