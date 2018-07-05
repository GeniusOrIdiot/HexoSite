---
title: 机器学习-线性回归标准方程的推导
date: 2018-06-06 20:39:12
categories: [Math, Machine Learning]
tags: [Machine Learning, Andrew Ng]
mathjax: true
---

`Pro.Ng`视频里的标准方程`Normal Equation`是怎么推导出来的？

<!-- more -->

---

乍看觉得一脸萌币有木有？……
$$
θ=(X^TX)^{-1}X^Ty
$$
用 $Octave$ 试了一下，确实是对的😮

到底是怎么一回事呢？🤔

我们知道：
$$
y=Xθ
$$
即：特征矩阵乘以特征系数得到结果集

按顺序做以下转换：
$$
X^Ty=X^TXθ……两边同左乘X^T
$$

$$
(X^TX)^{-1}X^Ty=(X^TX)^{-1}X^TXθ……两边同左乘((X^TX)^{-1})
$$

由于 $(X^TX)^{-1}$ 与 $X^TX$ 互逆，乘积为单位矩阵 $I$，于是：
$$
(X^TX)^{-1}X^Ty=(X^TX)^{-1}X^TXθ=Iθ=θ
$$
只能说—— $666$！