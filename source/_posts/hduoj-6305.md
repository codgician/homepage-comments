---
title: HDUOJ 6305 - RMQ Similar Sequence
date: 2018-07-26 16:33:23
tags: 
- ACM-ICPC
- Data Structure
- Cartesian Tree
- Mathematics
- Probability
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

对于序列 $A = \{ a_1, a_2, \dots, a_n \}$，定义 $\mathbf{RMQ}(A, l, r)$ 为原序列区间 $[l, r]$ 内最大值的下标（如果有多个最大值则为下标中的最小值）。如果两个序列 $A$、$B$ 满足：对于任意 $1 \le l \le r \le n$ 都满足 $\mathbf{RMQ}(A, l, r) = \mathbf{RMQ}(B, l, r)$，那么我们称序列 $A$、$B$ *RMQ相似*。

若已知序列 $A = \{ a_1, a_2, \dots, a_n \}$，对于序列 $B = \{b_1, b_2, \dots, b_n \}$ 定义序列 $B$ 的*权重* $W_B$：
$$
W_B = 
\begin{cases}
\sum\limits_{i = 1}^{n} b_i & A,B \text{ are RMQ similar} \\
0 & \text{otherwise}
\end{cases}
$$
现给定序列 $A$，且已知序列 $B$ 是由 $(0,1)$ 间随机实数组成的长度与 $A$ 相等的序列。试求 $W_B$ 的期望。期望值的形式是 $\frac{p}{q}$，而你需要输出 $pq^{-1} \pmod{10^9 + 7}$。

**数据范围**：

$1 \le n \le 10^6$

$1 \le a_i \le n$

所有测试数据 $n$ 之和不会超过 $3 \times 10^6$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6305)

# 分析

根据题目定义，如果两个序列 *RMQ相似*，通俗地讲就是两个序列每个区间内最大值的位置都相同。这令我们联想到**笛卡尔树**这一数据结构。

我们先简单回顾一下笛卡尔树的特性：

- 每个节点有两个键值：$index$ 和 $value$；
- $index$ 满足二叉搜索树性质，$value$ 满足堆性质。

这里如果我们把 $index$ 当作每个元素的下标，$value$ 当作每个元素的值，那么很容易可以发现两个序列 *RMQ 相似* 即两个序列的笛卡尔树同构。

我们先对 $A$ 建立一棵笛卡尔树。记笛卡尔树上以第 $i$ 个节点为根节点的子树的大小为 $size[i]$。在这个大小为 $size[i]$ 的子树中，对于任意一个节点都有 $\frac{1}{size[i]}$ 的概率成为根节点。那么根据乘法原理，$A$、$B$ 两个序列上构造的笛卡尔树同构的概率即：
$$
\prod\limits_{i = 1}^{n} \frac{1}{size[i]}
$$

然后由于 $B$ 中每个元素都是均匀地随机在 $(0, 1)$ 中取值， 所以取值的期望是 $\frac{1}{2}$。因此 $W_B$ 的期望即：
$$
\begin{aligned}
E_{W_B} & =  \prod\limits_{i = 1}^{n} \frac{1}{size[i]} \cdot \frac{1}{2} + (1 - \prod\limits_{i = 1}^{n} \frac{1}{size[i]}) \cdot 0 \\
& = \prod\limits_{i = 1}^{n} \frac{1}{2 \cdot size[i]}
\end{aligned}
$$
具体实现的时候要线性预处理逆元。

# 实现

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/6305/cartesian_tree.cpp)

# %%%

-  GodlikezzZ丶 - [杭电多校第一场 1008 HDU-6305 RMQ Similar Sequence（笛卡尔树）](https://blog.csdn.net/Lee_w_j__/article/details/81182212)