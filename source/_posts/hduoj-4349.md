---
title: HDUOJ 4349 - Xiao Ming's Hope
date: 2018-05-28 00:02:01
tags: 
- ACM-ICPC
- Mathematics
- Combinatorics
- Lucas' Theorem
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

给定 $n$，求组合数 $\binom{n}{0}, \binom{n}{1}, \dots, \binom{n}{n}$ 这 $n + 1$ 个数中值为奇数的个数。

**数据范围**：

$1 \le n \le 10^{18}$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4349)

# 分析

这个题很水，但是有点意思。

首先根据 **Lucas 定理**，我们可以得知：
$$
\begin{aligned}
\binom{n}{m} & \equiv \binom{n \bmod 2}{m \bmod 2}\cdot \binom{n \left. \right/ 2}{m \left. \right/ 2} \pmod 2 \\
& \equiv \binom{n \bmod 2}{m \bmod 2} \cdot \binom{n \left. \right/ 2 \bmod 2}{m \left. \right/ 2 \bmod 2} \cdot \binom{n \left. \right/ 4}{m \left. \right/ 4} \pmod 2 \\
\dots
\end{aligned}
$$
也就是说，记 $M_i$ 为 $m$ 二进制的第 $i$ 位，$N_i$ 为 $n$ 二进制的第 $i$ 位，同时记 $n$ 的二进制有 $k$ 位，则：
$$
\binom{n}{m} \equiv \prod_{i = 1}^{k} \binom{N_i}{M_i} \pmod 2
$$
显然，若要使得 $\binom{n}{m} \equiv 1 \pmod 2$，则对每一项都须有 $\binom{N_i}{M_i} \equiv 1 \pmod 2$。

如果 $N_i = 0$，那么当且仅当 $M_i = 0$ 时才会有 $\binom{N_i}{M_i} \equiv 1 \pmod 2$，而如果 $N_i = 1$，那么不论 $M_i$ 取何值都有 $\binom{N_i}{M_i} \equiv 1 \pmod 2$。因此，奇数的个数实际上就是所有 $N_i = 1$ 位上 $M_i$ 的组合种数。记 $n$ 的二进制值为 $1$ 的位数为 $h$，那么由乘法原理答案显然就为 $2^{h}$。

# 实现

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/4349/combinatorics_lucas.cpp)