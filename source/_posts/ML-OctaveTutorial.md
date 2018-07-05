---
title: 机器学习-Octave简易教程
date: 2018-06-09 21:21:47
categories: [Math, Machine Learning, Octave]
tags: [Machine Learning, Andrew Ng, Octave]
mathjax: true
---

`Octave`基本教程

```matlab
> eye(2)  % 单位矩阵
ans =

Diagonal Matrix

   1   0
   0   1
```

<!-- more -->

### 基础操作

---

使用`PS1('xxx')`改变你的命令符展示格式：

```matlab
octave:1> PS1('> ')
>
```

#### 基本数学运算

```matlab
> 5+6
ans =  11
> 3/4
ans =  0.75000
> 2^3
ans =  8
> sqrt(5)
ans =  2.2361
```

#### 逻辑运算

```matlab
> 1 == 2 % false
ans = 0
> 1 ~= 2 % not equals: true
ans = 1
> 1 && 0 % AND
ans = 0
> 1 || 0 % OR
ans = 1
> xor(1,0)
ans = 1
```

#### 声明变量

```matlab
> a = 3
a =  3
> a = 3;  % 利用;符号控制是否输出结果
> a  % 声明过的变量可以直接输入变量名来输出
a =  3
> a = pi;
> disp(a)  % 直接输出值，不显示变量名
3.1416
> disp(sprintf('2 decimals: %0.2f', a))  % 使用sprintf按需要输出
2 decimals: 3.14
> disp(sprintf('6 decimals: %0.6f', a))
6 decimals: 3.141593
> % 此外：'%0.2f'是C的语法。'%w.h f'：表示输出的总宽度是w小数点后保留h位浮点数(f)类型数值，如果实际长度大于他想控制输出的长度w，则还是按实际长度输出
```

#### 声明矩阵

```matlab
> A = [1 2;3 4;5 6]
A =

   1   2
   3   4
   5   6

> A = [1,2;3,4;5,6]
A =

   1   2
   3   4
   5   6
   
> A = [1,2;
> 3,4;
> 5,6]  % 使用;时，你还可以换行以矩阵的格式输入
A =

   1   2
   3   4
   5   6

> v = [1,2,3]  % vector
v =

   1   2   3

> v = [1;2;3]  % column vector
v =

   1
   2
   3

> v = 1:0.2:2  % 以0.2为增量从1到2的数构成的矩阵
v =

    1.0000    1.2000    1.4000    1.6000    1.8000    2.0000

> v = 1:6  % 默认增量为1
v =

   1   2   3   4   5   6

> ones(2,3)  % 全部为1的矩阵
ans =

   1   1   1
   1   1   1

> zeros(3,2)  % 全部为0的矩阵
ans =

   0   0
   0   0
   0   0

> eye(2)  % 单位矩阵
ans =

Diagonal Matrix

   1   0
   0   1

> w = rand(1,3)  % 全部是0~1随机值的矩阵
w =

   0.74666   0.34772   0.21182

> w = randn(1,6)  % 高斯随机变量
w =

  -0.99990  -0.10855  -0.86680  -0.94371  -0.87955   0.66501

```

#### 矩阵的大小

```matlab
> A = [1 2; 3 4; 5 6]
A =

   1   2
   3   4
   5   6

> size(A)
ans =

   3   2

> size(A,1)  % 第一个维度对应的大小 3
ans =  3
> size(A,2)  % 第二个维度对应的大小 2
ans =  2
> size(A,3)  % 虽然没有考虑过更高阶的矩阵，但是默认更高的维度对应元素都是1
ans =  1
> size(A,4)
ans =  1
> V = [1,2,3,4]
V =

   1   2   3   4

> length(V)  % 向量的长度
ans =  4
> length(A)  % 对矩阵而言就是最大的那个维度的长度
ans =  3
```

#### 画图

```matlab
> w = 6 + sqrt(10)*(randn(1,10000));  % 这是一个有10000个数的矩阵
> hist(w)  % 展示如下
> hist(w,50)  % 以50条直方展示的直方图
```

![高斯随机数正态分布图](/images/ML-Octave/1528553786815.jpg)

