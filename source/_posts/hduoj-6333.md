---
uuid: 1570536a-d854-11e8-86e6-d745f7e6a79d
title: HDUOJ 6333 - Harvest of Apples
date: 2018-08-04 19:57:23
updated: 2018-08-04 19:57:23
tags: 
  - Competitive Programming
  - Mathematics
  - Combinatorics
  - Partitioning
  - Mo's Algorithm
  - HDUOJ
category: Solutions
---

# 题面

给定 $T$ 组 $n, m$，求：

$$
\sum\limits_{i = 0}^{m} \binom{n}{i}
$$

结果对 $10^9 + 7$ 取模。

**数据范围**：

$1 \le T \le 10^5$

$1 \le m \le n \le 10^5$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6333)

# 分块

## 分析

虽然经过 $\mathcal{O}(n)$ 的预处理后我们可以以 $\mathcal{O}(1)$ 算出 $\binom{n}{i}$，但是 $\mathcal{O}(Tn)$ 的复杂度显然是不能被接受的。

那怎么办…… 我们希望复杂度能够降至 $\mathcal{O}(T\sqrt{n})$ 左右…… 分块？

分块的思想即**大段维护，局部朴素**。假设块长为 $L$，令数组 $A[i][j]$ 代表 $\sum\limits_{k = 0}^{L \cdot j} \binom{i}{k}$，即前 $j$ 块的前缀和。

这样一来，对于给定的 $n, m$，令 $S = \lfloor \frac{A}{L} \rfloor$，我们可以把待求的值分为两部分：

$$
\sum\limits_{i = 0}^{m} \binom{n}{i} = A[n][S] + \sum\limits_{i = L \cdot S + 1}^{m} \binom{n}{i}
$$

取 $L = \sqrt{n}$ 左右的话，每一次计算的复杂度就变成了 $\mathcal{O}(\sqrt{n})$ 级别。接下来我们需要在 $\mathcal{O}(\sqrt{n})$ 级别的复杂度内完成对 $A$ 的预处理。

我们考虑 $A[i][j]$ 和 $A[i - 1][j]$ 间能否在 $\mathcal{O}(1)$ 内完成转移。

首先，我们知道：

$$
\binom{n}{m} = \binom{n - 1}{m} + \binom{n - 1}{m - 1}
$$

那么相应地：

$$
\begin{aligned}
\sum\limits_{i = 1}^{m} \binom{n}{i} & = \binom{n}{0} + \binom{n}{1} + \dots + \binom{n}{m} \\
& = 2 \cdot \binom{n - 1}{0} + 2 \cdot \binom{n - 1}{1} + \dots + 2 \cdot \binom{n - 1}{m - 1} + \binom{n - 1}{m} \\
& = 2 \cdot \sum\limits_{i = 0}^{m} \binom{n - 1}{i} - \binom{n - 1}{m}
\end{aligned}
$$

所以也就有：

$$
A[i][j] = 2 \cdot A[i - 1][j] - \binom{i - 1}{(j + 1) \cdot L - 1}
$$

其中 $(j + 1) \cdot L - 1$ 也就是第 $j$ 块的最后一个元素。而这个组合数我们可以用预处理阶乘和逆元的方式在 $\mathcal{O}(1)$ 内求得。

另外需要注意的是，在 $L|i$ 时（也就是末尾出现了一个新的块时），需要手动处理一下： $A[i][\lfloor \frac{i}{L} \rfloor] = A[i][\lfloor \frac{i}{L} \rfloor - 1] + 1$。否则如果新出现的这一块一直是 $0$ 的话后续转移会出问题。

另外，如果本题按照 $\sqrt{N}$ 取块长三百左右的话貌似会 MLE…… 我是取四百左右过的。

## 实现

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/HDUOJ/6333/combinatorics_partitioning.cpp)

# 莫队

## 分析

当得知这道题还可以莫队时…… orz。

首先我们不妨记 $\mathcal{S}(n, m) = \sum\limits_{i = 0}^{m} \binom{n}{i}$。

由上我们已经得到了：

$$
\mathcal{S}(n, m) = 2 \cdot \mathcal{S}(n - 1, m) - \binom{n - 1}{m}
$$

那么反过来：

$$
\mathcal{S}(n, m) = \frac{1}{2} \cdot (\mathcal{S}(n + 1, m) + \binom{n}{m})
$$

好了，$n$ 的转移方式我们已经得到了。那么 $m$ 的转移呢？这不是很显然吗……

$$
\mathcal{S}(n, m) = \mathcal{S}(n, m - 1) + \binom{n}{m}
$$

$$
\mathcal{S}(n, m) = \mathcal{S}(n, m + 1) - \binom{n}{m}
$$

现在 $n$ 和 $m$ 的 $\mathcal{O}(1)$ 转移方式都有了…… 我们好像真的可以用莫队了诶。真心佩服出题人的脑洞。

## 实现

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/HDUOJ/6333/combinatorics_mo's_algorithm.cpp)
