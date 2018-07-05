---
title: 机器学习-分类回归算法
date: 2018-06-30 15:52:38
categories: [Math, Machine Learning, Octave]
tags: [Machine Learning, Andrew Ng, Octave]
mathjax: true
---

以数字 $0\sim9$ 的手写识别为例，演示分类回归。

<!-- more -->

### 前期准备

##### 目前已经开放的情报

- 逻辑回归

利用 $Sigmoid$ 函数将 $h(x)$ 放缩到 $0\sim1$ 之间，以实现对分类结果是`是/否`的判断，从而通过回归算法梯度下降来拟合出最优的特征参数 $\theta$ ，详见：[机器学习-逻辑回归](https://geniusoridiot.github.io/2018/06/21/ML-LogisticRegression/)

