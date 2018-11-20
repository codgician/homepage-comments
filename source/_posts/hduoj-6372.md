---
uuid: 5e5ca97a-d854-11e8-af6d-c7495555ed30
title: HDUOJ 6372 - sacul
date: 2018-08-10 16:02:23
updated: 2018-08-10 16:02:23
tags: 
  - Competitive Programming
  - Mathematics
  - Combinatorics
  - Lucas' Theorem
  - HDUOJ
category: Solutions
---

# 题面

某巨犇发现了一种叫做~~海绵宝宝~~ $\text{HMBB}$ 的神奇矩阵。

给定 $i$，记 $p$ 是第 $i$ 个质数。

矩阵 $\text{HMBB}_{n}$ 的大小为 $p^n \times p^n$，同时：

$$
\text{HMBB}_{n}[i][j] =
\begin{cases}
0 & \binom{i}{j} \equiv 0 \pmod p \\
1 & \binom{i}{j} \not \equiv 0 \pmod p \\
\end{cases}
$$

记 $F(n, k)$ 为矩阵 $(\text{HMBB}_n)^k$ 中所有元素之和。
给定 $n, k$，试求：

$$
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{k} F(i, j)
$$
并且结果需对 $10^9 + 7$ 取模。

**数据范围**：

$0 < n \le 10^9$

$0 < c, k \le 10^5$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6372)

# 分析

在比赛的时候立马联想到了之前做过的一道题：[HDUOJ 4349 - Xiao Ming's Hope](https://blog.codgician.pw/2018/05/27/hduoj-4349/)。

考虑 Lucas 定理：

$$
\begin{aligned}
\binom{n}{m} & \equiv \binom{n \mod p}{m \mod p} \cdot \binom{n / p}{m / p} \pmod p \\
 & \equiv \binom{n \mod p}{m \mod p} \cdot \binom{n / p \mod p}{m / p \mod p} \cdot \binom{n / p^2}{m / p^2} \pmod p \\
 & \dots \\
 \end{aligned}
$$

不难发现其本质就是把 $n$ 和 $m$ 按照 $p$ 进制进行拆位，对每一位计算组合数后再乘起来。

严格地说，记 $N_i$ 为 $n$ 在 $p$ 进制下的第 $i$ 位，$M_i$ 为 $m$ 在 $p$ 进制下的第 $i$ 位，则有：

$$
\binom{n}{m} \equiv \prod \binom{N_i}{M_i} \pmod p
$$

接下来我们来看看什么时候存在 $\binom{n}{m} \equiv 0 \pmod p$。

首先，对于上面连乘式中的第 $i$ 项，显然有 $0 \le N_i, M_i < p$。

- 若 $N_i \ge M_i$，那么 $\binom{N_i}{M_i}$ 一定是整数，又由于 $N_i, M_i < p$，所以模 $p$ 一定是正数（不可能存在 $p$ 这一因子）；
- 若 $N_i < M_i$，那么 $\binom{N_i}{M_i} = 0$，模 $p$ 自然为 $0$。

**换句话说，对于 $\binom{n}{m} \mod p$，若 $n$ 在 $p$ 进制下的每一位都不小于 $m$ 在 $p$ 进制下的对应位，则有 $\binom{n}{m} \not \equiv 0 \pmod p$**。

貌似题目标题反过来读就是题解诶，然而比赛的时候想到这里后我就不会了 QAQ。

---

另外注意到计算 $F(n, k)$ 的过程中涉及矩阵乘法，我又联想到之前做过的一道题：[HDUOJ 6331 - Walking Plan](https://blog.codgician.pw/2018/08/07/hduoj-6331/)。我们可以借助那道题的思想，把矩阵乘法和图论知识联系在一起。

既然 $\text{HMBB}$ 是一个 $01$ 矩阵，我们不妨把它联想成一个可达性矩阵，由此可以将其**对应成一张有向无权图**。进一步，$\text{HMBB}[i][j]$ 代表从 $i$ 出发，走 $1$ 步到达 $j$ 的方案数。再进一步，$(\text{HMBB}[i][j])^k$ 代表从 $i$ 出发，恰好走 $k$ 步到达 $j$ 的方案数。

根据上面根据 Lucas 定理得到的结论，如果 $i$ 在 $p$ 进制下每一位都不小于 $j$ 在 $p$ 进制下的对应位，那么我们就建一条有向边：$i \to j$。**于是 $F(n, k)$ 就变成了在包含 $p^n$ 个点的这样的图中恰好走 $k$ 步从任意点出发到达任意点的方案总数**。

---

现在我们考虑恰好走 $j$ 步的路径，那么显然这条路径上有 $j + 1$ 个点，同时这条路径上每一个点都满足：每个数的 $p$ 进制上的每一位不小于上一个数的对应位。这又回到了一个经典排列组合问题：

> 现有 $k$ 个数：$x_1, x_2, \dots x_k$，已知对于每个 $x$ 都有 $0 \le x < p$，试问满足 $x_1 \le x_2 \le \dots \le x_k$ 的排列方案有多少种？

我好像又联想到了之前做过的一道题：[HDUOJ 3037 - Saving Beans](https://blog.codgician.pw/2018/05/26/hduoj-3037/)。用其中类似的思想可以解决这一问题：

> 解：原问题可转化为 $x_1 < (x_2 + 1) < (x_3 + 2) < \dots < (x_k + k - 1)$ 的方案数。换句话说，就是记 $y_i = x_i + i - 1$，$y_1 < y_2 < \dots < y_k$ 的方案数，其中 $0 \le y < p + k - 1$。显然等效于 $p + k - 1$ 种值里面取 $k$ 种不同的值：
> $$\binom{p + k - 1}{k} = \binom{p + k - 1}{p - 1}$$

所以说对于每一位实际上就是上面公式里面带入 $k = j + 1$，我们可采取的方案数都为：

$$
\binom{p + (j + 1) - 1}{p - 1} = \binom{j + p}{p - 1}
$$

记这些数的 $p$ 进制都有 $i$ 位，显然这 $i$ 个位之间是相对独立的，因此方案数为：

$$
F(i, j) = \binom{j + p}{p - 1} ^ i
$$

那么题目所要求的值即为：

$$
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{k} \binom{j + p}{p - 1}^i = \sum\limits_{j = 1}^{k} \sum\limits_{i = 1}^{n} \binom{j + p}{p - 1}^i
$$

# 实现

由上述分析我们已经知道答案即：

$$
\sum\limits_{j = 1}^{k} \sum\limits_{i = 1}^{n} \binom{j + p}{p - 1}^i
$$

其中后半部分可看作一个等比数列，用等比数列公式即可直接求出（需要注意特判公比为 $1$ 的情况）。

如果我们预处理阶乘的话，求一个组合数的的复杂度为 $\mathcal{O}(\log{N})$ 级别，因此总复杂度为 $\mathcal{O}(N\log{N})$ 级别。

另外由实践得知，第 $10^5$ 个质数为 $1299709$，因此我们在处理阶乘的时候至少需要处理至 $1299709 + 10^5$。

[完整参考代码](https://github.com/codgician/ICPC/blob/master/HDUOJ/6372/combinatorics_lucas.cpp)

# %%%

- FZDX - [2018 Multi-University Training Contest 6 solutions BY 福州大学](http://bestcoder.hdu.edu.cn/blog/2018-multi-university-training-contest-6-solutions-by-%E7%A6%8F%E5%B7%9E%E5%A4%A7%E5%AD%A6/)