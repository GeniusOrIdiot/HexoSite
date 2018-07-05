---
title: 机器学习-线性代数之矩阵
date: 2018-06-03 10:31:33
categories: [Math, Machine Learning]
tags: [Machine Learning, Andrew Ng, Matrics]
mathjax: true
---

需要更深入地学习机器学习及其算法，线性代数的一些基础的知识技能是有必要的。

<!-- more -->

---



### 矩阵-Matrics

#### 矩阵

> 如果你知道数组，或许可以帮助理解矩阵。

比如：
$$
A=\left [ 
 \begin{matrix} 
1 \\\\ 2 \\\\ 3
 \end{matrix} 
 \right ]
$$
这就是一个矩阵，也可以说是一个数组，其中的每一个元素，可以用序号表示：$ A[\ i\ ] $

在`Pro.Ng`机器学习课程中，我们使用 $1$ 开始的序列模式`1-indexed`（即，$i$ 从 $1$ 开始），尽管很多编程语言会使用 $0$ 开始的序列模式`0-indexed`。

#### 矩阵的行`Rows`和列`Cols`

> 矩阵就像一个二维数组，行和列构成了矩阵的大小`Size`。

比如：
$$
B=\left [ 
 \begin{matrix} 
1 & 2 \\\\ 
3 & 4 \\\\
5 & 6
 \end{matrix} 
 \right ]
$$
这是一个$3×2$的矩阵，或者说是一个行维有$3$个元素，列维有$2$个元素的数组，第$2$行第$1$列的元素可以表示为：$B\_{21}$，或者是：$B[1][0]$（数组形式，`0-indexed`）

只有一列的矩阵，又称之为__向量__ ($Vector$)，如：
$$
v=\left [ 
 \begin{matrix} 
1 \\\\ 2 \\\\ 3
 \end{matrix} 
 \right ]
$$

> $PS:$ 虽然形式上类似于二维数组，但是实际上，矩阵可以表达的维度是没有限制的。

#### 在Octave中练习矩阵

> 下面就是一些练习，在 $Octave$ 里试试吧！

```matlab
% The ; denotes we are going back to a new row.
A = [1, 2, 3; 4, 5, 6; 7, 8, 9; 10, 11, 12]

% Initialize a vector 
v = [1;2;3] 

% Get the dimension of the matrix A where m = rows and n = columns
[m,n] = size(A)

% You could also store it this way
dim_A = size(A)

% Get the dimension of the vector v 
dim_v = size(v)

% Now let's index into the 2nd row 3rd column of matrix A
A_23 = A(2,3)

% Make sure you understand why we got that result
```



### 矩阵运算

> 机器学习算法里有着巨大的运算量，普通的运算方式必然会带来巨大的开销，利用矩阵，不仅可以优化运算，直观上的美观以及阅读性都能得到很大的提升。不过首先，我们需要知道矩阵是如何运算的。

#### 矩阵的加法

> 必须是相同大小的矩阵，才可以进行加法运算。

比如：
$$
\left [ 
 \begin{matrix} 
1 & 2 \\\\ 3 & 4
 \end{matrix} 
 \right ]+
 \left [ 
 \begin{matrix} 
5 & 6 \\\\ 7 & 8
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
1+5 & 2+6 \\\\ 3+7 & 4+8
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
6 & 8 \\\\ 10 & 12
 \end{matrix} 
 \right ]
$$
对应位置的元素相加，得到与原来两个矩阵大小`Size`相同的矩阵。

大小不同的矩阵无法执行加法运算：
$$
\left [ 
 \begin{matrix} 
1 \\\\ 2 \\\\ 3
 \end{matrix} 
 \right ]+
 \left [ 
 \begin{matrix} 
4 \\\\ 5
 \end{matrix} 
 \right ]...×
$$
或者是：
$$
\left [ 
 \begin{matrix} 
1 \\\\ 2 \\\\ 3
 \end{matrix} 
 \right ]+
 \left [ 
 \begin{matrix} 
4 & 5 & 6
 \end{matrix} 
 \right ]...×
$$
都是不允许的。

#### 矩阵的数乘

> 相同的 $n$ 个矩阵相加，即矩阵的数乘。

比如：
$$
2×\left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]+
 \left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
2 & 4 & 6
 \end{matrix} 
 \right ]
$$
根据规律，可以对每一个元素数乘：
$$
2×\left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
2×1 & 2×2 & 2×3
 \end{matrix} 
 \right ]
$$

#### 在Octave中练习矩阵运算（一）

下面是一些例子，请用 $Octave$ 尝试一下。

```matlab
% Initialize matrix A and B 
A = [1, 2, 4; 5, 3, 2]
B = [1, 3, 4; 1, 1, 1]

% Initialize constant s 
s = 2

% See how element-wise addition works
add_AB = A + B 

% See how element-wise subtraction works
sub_AB = A - B

% See how scalar multiplication works
mult_As = A * s

% Divide A by s
div_As = A / s

% What happens if we have a Matrix + scalar?
add_As = A + s

% Make sure you understand why we got that result
```

#### 矩阵的乘法

> 矩阵乘法要求相乘的矩阵 $A×B$ ，$A的列数=B的行数$，顺序不可逆。

比如：
$$
\left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]×
 \left [ 
 \begin{matrix} 
4 & 5 \\\\ 6 & 7 \\\\ 8 & 9
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
1×4+2×6+3×8 & 1×5+2×7+3×9
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
40 & 46
 \end{matrix} 
 \right ]
$$
乘得的矩阵有 $A的行数$ 和 $B的列数$ ，亦即：
$$
A\_{m×p}×B\_{p×n}=P\_{m×n}
$$


> 需要注意的是：矩阵不满足交换律，即使两个矩阵行列数均相等，其相乘时的顺序也是不可逆的。
>
> 但是满足结合律，当然，结合律中无需行列数均等的限制，但依然需要保证满足乘法所需的行列数的要求（左矩阵的列数=右矩阵的行数）。

$$
A\_{m×m}×B\_{m×m} \ne B\_{m×m}×A\_{m×m}
$$

$$
A×B×C = A×(B×C)
$$

#### 在Octave中练习矩阵运算（二）

练习一下矩阵的乘法运算吧！

```matlab
% Initialize matrix A 
A = [1, 2, 3; 4, 5, 6;7, 8, 9] 

% Initialize vector v 
v = [1; 2; 3] 

% Multiply A * v
Av = A * v

% Initialize random matrices B and C 
B = [1,2;4,5]
C = [1,1;0,2]

% Compute B*C 
BC = B*C 

% Is it equal to C*B? 
CB = C*B

% Note that BC != CB
% Make sure you understand why we got that result
```



#### 单位矩阵-Identity Matrics

> 行列数相同，左上至右下对角线上元素都是 $1$ ，其它元素都是 $0$ 的矩阵叫做单位矩阵($Identity\ Matrics$)。

比如：
$$
I\_{3×3} = \left [ 
 \begin{matrix} 
1 & 0 & 0 \\\\
0 & 1 & 0 \\\\
0 & 0 &1
 \end{matrix} 
 \right ]
$$
这是一个 $3$ 阶单位矩阵，任何列数为 $3$ 的矩阵乘以该单位矩阵，都不变。

比如：
$$
\left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]×
 \left [ 
 \begin{matrix} 
1 & 0 & 0 \\\\
0 & 1 & 0 \\\\
0 & 0 &1
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
1 & 2 & 3
 \end{matrix} 
 \right ]
$$
或是：
$$
\left [ 
 \begin{matrix} 
1 & 2 & 3 \\\\
4 & 5 & 6
 \end{matrix} 
 \right ]×
  \left [ 
 \begin{matrix} 
1 & 0 & 0 \\\\
0 & 1 & 0 \\\\
0 & 0 &1
 \end{matrix} 
 \right ]=
 \left [ 
 \begin{matrix} 
1 & 2 & 3 \\\\
4 & 5 & 6
 \end{matrix} 
 \right ]
$$
根据矩阵乘法的规则尝试一下便可以推导出以上结果。

在 $Octave$ 中，单位矩阵可以简单声明 ，如：$eye(3)$，即声明了一个 $3$ 阶单位矩阵

#### 矩阵的逆-Inverse

> “逆“的概念，我觉得和“倒数”比较像：矩阵与该矩阵的逆的乘积等于单位矩阵 => 一个数与一个数的倒数乘积等于 $1$。

求逆的方法并不像求倒数那样简单，姑且不用考虑这个……因为 $Octave$ 已经有了求逆的函数： $inv(M)$

因为上面的概念，矩阵的逆，有时候又表示为：$A^{-1}$

> $PS:$ 只有方阵`Square Matrics`才有逆。方阵：行数与列数相同的矩阵

#### 矩阵的转置-Transpose

> 将一个矩阵沿着左上角$45º$对角线将其翻折，即得到其转置。

比如：
$$
\left [ 
 \begin{matrix} 
1 & 2 & 3 \\\\
4 & 5 & 6
 \end{matrix} 
 \right ]^T=
 \left [ 
 \begin{matrix} 
1 & 4 \\\\
2 & 5 \\\\
3 & 6
 \end{matrix} 
 \right ]
$$
在 $Octave$ 中，转置的符号是 $'$ ，如：$A'$

#### 在Octave中练习矩阵运算（三）

前面接触了单位矩阵、逆和转置，现在在 $Octave$ 中试试吧！

```matlab
% Initialize random matrices A
A = [1,2;4,5]

% Initialize a 2 by 2 identity matrix
I = eye(2)

% The above notation is the same as I = [1,0;0,1]

% What happens when we multiply I*A ? 
IA = I*A 

% How about A*I ? 
AI = A*I 

% Note that before AB != BA but IA = AI

% Initialize matrix B
B = [1,2,0;0,5,6;7,0,9]

% Transpose B
B_trans = B' 

% Take the inverse of B
B_inv = inv(B)

% What is B^(-1)*B? 
B_invB = inv(B)*B
% Make sure you understand why we got that result
```

> $PS:$ 值得注意的是：满足 $AI = IA$ 的矩阵 $A$ 也必须是方阵。
>
> 由此可以细化单位矩阵部分的结论：

$$
A\_{m×n}×I\_n=A\_{m×n}=I\_m×A\_{m×n}
$$

> 左乘和右乘的单位矩阵存在差异。

