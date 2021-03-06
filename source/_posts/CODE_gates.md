---
title: 《CODE》读书笔记之一：门与逻辑电路
tags: 《CODE》笔记整理
date: 2018-03-01
---
逻辑运算、逻辑电路、逻辑门……计算机的一些细节设计是怎么来的？
<!-- more -->

有人认为`门`是以`Bill Gates`(gate，即大门)的名字来命名的，其实只是巧合。`门`可以理解为电路的门或电流的阀门。

---
## 继电器
- 继电器像开关一样，但它优于开关之处在于，它可以被其它继电器触发。

    ![继电器](/images/2018-02-11/继电器.png)
---
## 逻辑门(Logic Gates)
连接继电器是建立逻辑门的关键。

- 反向器：
  - 反向器并不能称之为`门`，一般使用两个以上的输入才能称之为门
  - 反向器的作用正如它的名字一样，当输入0时，输出为1；当输入1时，输出为0
  - 习惯上称为非门，符号是`^`，命名为`NOT`
- 与门
  - 与门的符号是`&`，命名为`AND`
- 或门
  - 或门的符号是`|`，命名为`OR`
- 与非门
  - 符号：`^&`，命名：`NAND`
- 或非门
  - 符号：`^|`，命名：`NOR`

| 逻辑门 | 电路                                                         | 图示                                                         | 真值表                                                       |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 非门   | ![非门电路](/images/2018-02-11/非门电路.png) | ![非元件](/images/2018-02-11/非元件.png) |                                                              |
| 与门   | ![与电路](/images/2018-02-11/与电路.png) | ![与元件](/images/2018-02-11/与元件.png) | ![与真值](/images/2018-02-11/与真值.png) |
| 或门   | ![或电路](/images/2018-02-11/或电路.png) | ![或元件](/images/2018-02-11/或元件.png) | ![或真值](/images/2018-02-11/或真值.png) |
| 与非门 | ![与非电路](/images/2018-02-11/与非电路.png) | ![与非元件](/images/2018-02-11/与非元件.png) | ![与非真值](/images/2018-02-11/与非真值.png) |
| 或非门 | ![或非电路](/images/2018-02-11/或非电路.png) | ![或非元件](/images/2018-02-11/或非元件.png) | ![或非真值](/images/2018-02-11/或非真值.png) |

---

## 逻辑运算
逻辑门实现的依据是逻辑运算

利用上述几个基本的逻辑门，可以实现一些比较复杂的逻辑运算

- 异或：__`XOR`__ $(exclusive \  OR)$

  | XOR  |  0   |  1   |
  | :--: | :--: | :--: |
  |  0   |  0   |  1   |
  |  1   |  1   |  0   |

- 同或：__`XNOR`__ $(exclusive \  NOR)$

  | XNOR |  0   |  1   |
  | :--: | :--: | :--: |
  |  0   |  1   |  0   |
  |  1   |  0   |  1   |

  