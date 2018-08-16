---
title: HDUOJ 6397 - Character Encoding
date: 2018-08-16 15:05:23
tags: 
- ACM-ICPC
- Mathematics
- Combinatorics
- Inclusion-Exclusion Principle 
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

给定 $n, m, k$，求不定方程：
$$
\sum\limits_{i = 1}^{m} x_i = k \ (x_i \in Z, 0 \le x_i < n)
$$
的解的个数。

**数据范围**：

$1 \le n, m \le 10^5$

$0 \le k \le 10^5$

所有数据 $n, m, k$ 之和不会超过 $5 \times 10^6$。

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6397)

# 分析

首先，如果题目没有给出 $x_i < n$ 这一限制，我们可以用隔板法（参考 [HDUOJ 3037 - Saving Beans](https://blog.codgician.pw/2018/05/26/hduoj-3037/)）求出解的种数：
$$
\binom{k + m - 1}{m - 1}
$$

---

但是现在我们有 $x_i < n$ 这一限制条件，因此我们需要把存在 $x_i \ge n$ 的解剔除掉。我们考虑使用**容斥原理**。

为了方便叙述，我们以 $m = 3$ 的情况为例，画出如下韦恩图：

![韦恩图](hduoj-6397/venn.png)

我们所要求的就是 $x_1, x_2, x_3 < n$ 的部分（即上图中的 $S_0$）。

我们先用容斥原理求出存在 $x_i$ 不满足上限的全集的大小（即 $|S_1 \cup S_2 \cup S_3|$）：
$$
\begin{aligned}
|\bigcup\limits_{i = 1}^{3} S_i| &= (|S_1| + |S_2| + |S_3|) - (|S_1 \cup S_2| + |S_1 \cup S_3| + |S_2 \cup S_3|) + |S_1 \cup S_2 \cup S_3|  \\
& = \sum\limits_{i = 1}^{3}|S_i| - \sum\limits_{1 \le i < j \le 3}|S_i \cup S_j| + \sum\limits_{1 \le i < j < k \le 3} |S_i \cup S_j \cup S_k|
\end{aligned}
$$


那么我们所要求的答案就是所有方案数减去存在 $x_i$ 不满足上限的方案数：
$$
|S_0| = |S_0 \cup S_1 \cup S_2 \cup S_3| - |S_1 \cup S_2 \cup S_3|
$$
我们可以轻松从 $m = 3$ 推广到 $m$ 为任意值的情况：
$$
\begin{aligned}
|S_0| & = |\bigcup\limits_{i = 0}^{m} S_i| - |\bigcup\limits_{i = 1}^{m} S_i| \\
& = \binom{k + m - 1}{m - 1} - \left[\sum\limits_{i = 1}^{m} |S_i| - \sum\limits_{1 \le i < j \le m} |S_i \cup S_j| + \dots + (-1)^{m + 1}|S_1 \cup \dots \cup S_m| \right]
\end{aligned}
$$

---

接下来我们需要使用组合数学的相关知识把上述公式中的每一项表示出来。

记 $F(p)$ 代表从 $m$ 个 $x_i$ 中选取 $p$ 个来违反上界限值的种数，那么上述公式可改写为：
$$
\begin{aligned}
|S_0| & = \binom{k + m - 1}{m - 1} - \left[ F(1) - F(2) + \dots + (-1)^{m + 1} F(m) \right] \\
& = \binom{k + m - 1}{m - 1} + \sum\limits_{i = 1}^{m} (-1)^i \cdot F(i)
\end{aligned}
$$
我们来思考如何表示出 $F(i)$。记选出违反上界限制的 $p$ 个未知量为 $x_1, x_2, \dots x_p$，则：


$$
\begin{aligned}
x_1 + x_2 + \dots + x_m & = k\\
\Rightarrow  (x_1 - n) + (x_2 - n) + \dots + (x_p - n) + x_{p + 1} + \dots + x_m & = k - p \cdot n
\end{aligned}
$$
由此一来我们选出的 $p$ 个不违反上界限制的变量都满足上界限制了（当然未选出的变量仍有可能违反上界限制）。套用本文开头所述的公式得解的组数为：$\binom{k - p \cdot n + m - 1}{m - 1}$，而在 $m$ 个 $x_i$ 中任选 $p$ 个的方案为 $\binom{m}{p}$， 由乘法原理得：
$$
F(p) = \binom{m}{p} \cdot \binom{k - p \cdot n + m - 1}{m - 1}
$$

我们将其带入上面我们得到的公式就可以求得解了。

# 实现

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/6397/combinatorics_inclusion_exclusion_principle.cpp)