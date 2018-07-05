---
title: 机器学习-Octave简易教程(2)
date: 2018-06-10 19:05:44
categories: [Math, Machine Learning, Octave]
tags: [Machine Learning, Andrew Ng, Octave]
mathjax: true
---

`Octave`基本教程

```matlab
> [1,2,3] * [3;2;1]
ans =  10
> [1,2,3] .* [3;2;1]
ans =
   3   6   9
   2   4   6
   1   2   3
```

<!-- more -->

### 数据操作

#### 从文件加载数据

```matlab
> cd xxx/xxx/xxx	# 到你存放数据文件的目录
> ls		# 查看当前目录下的文件
ex1data1.txt	ex1data2.txt
> load ex1data1.txt	# 加载该文件数据，或者：
> load('ex1data1.txt')
```

#### 检查当前数据

```matlab
> who
Variables in the current scope:

a         ans       b         ex1data1

> whos
Variables in the current scope:

   Attr Name          Size                     Bytes  Class
   ==== ====          ====                     =====  =====
        a             3x3                         72  double
        ans           1x73                        73  char
        b             3x3                         72  double
        ex1data1     97x2                       1552  double

Total is 285 elements using 1769 bytes
```

#### 清除数据

```matlab
> clear a	# 只清除a
> whos
Variables in the current scope:

   Attr Name          Size                     Bytes  Class
   ==== ====          ====                     =====  =====
        ans           1x73                        73  char
        b             3x3                         72  double
        ex1data1     97x2                       1552  double

Total is 276 elements using 1697 bytes
> clear		# 清除全部
> whos		# 什么都没有
```

#### 选取数据

```
> 
```

