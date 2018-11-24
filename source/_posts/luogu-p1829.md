---
uuid: be05f410-ecbf-11e8-8505-57494f4f91cd
title: Luogu P1829 - Crash 的数字表格
date: 2018-11-21 19:15:10
updated: 2018-11-21 19:15:10
tags:
  - Competitive Programming
  - Mathematics
  - Number Theory
  - Luogu
category: Solutions
---

# 题面

给定 $n, m$，试求：

$$
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \operatorname{lcm}(i, j)
$$

答案对 $20101009$ 取模。

**数据范围**：

$1 \le n,m \le 10^7$

[题目链接](https://www.luogu.org/problemnew/show/P1829)

# 分析

显然我们需要往莫比乌斯函数的方向考虑。因此，我们要把 $\operatorname{lcm}(i, j)$ 换作使用 $\gcd(i, j)$ 表示。

为了方便，下文中假设 $n \le m$（如果 $n > m$ 那么两者交换一下就好了）。

$$
\begin{aligned}
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \operatorname{lcm}(i, j) = & \sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \frac{ij}{\gcd(i, j)}
\end{aligned}
$$

我们发现 $\gcd(i, j)$ 位于分母，这十分不好处理。因此我们在这里介绍一个技巧，令 $\gcd(i, j) =k $ 并枚举 $k$ 从而使得 $\gcd(i, j)$ 离开分母：

$$
\begin{aligned}
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \operatorname{lcm}(i, j) = & \sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \sum\limits_{k = 1}^{n} \frac{ij}{k} [\gcd(i. j) = k] \\
= & \sum\limits_{k = 1}^{\min(n, m)} \sum\limits_{i = 1}^{\left\lfloor \frac{n}{k} \right\rfloor} \sum\limits_{j = 1}^{\left\lfloor \frac{m}{k} \right\rfloor} ijk[\gcd(i, j) = 1]
\end{aligned}
$$

终于出现了我们最喜闻乐见的 $[\gcd(i, j) = 1]$。我们可以依据我上一篇文章 [浅谈莫比乌斯反演](https://blog.codgician.pw/2018/11/18/mobius-inversion-formula/) 应用一节中提到的方法对其进行变换：

$$
\begin{aligned}
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \operatorname{lcm}(i, j) = & \sum\limits_{k = 1}^{n} \sum\limits_{i = 1}^{\left\lfloor \frac{n}{k} \right\rfloor} \sum\limits_{j = 1}^{\left\lfloor \frac{m}{k} \right\rfloor} ijk \sum\limits_{d|\gcd(i, j)} \mu(d) \\
= & \sum\limits_{k = 1}^{n} \sum\limits_{i = 1}^{\left\lfloor \frac{n}{k} \right\rfloor} \sum\limits_{j = 1}^{\left\lfloor \frac{m}{k} \right\rfloor} ijk \sum\limits_{d = 1}^{\left\lfloor \frac{n}{k} \right\rfloor} \mu(d) [d | \gcd(i, j)] \\
= & \sum\limits_{k = 1}^{n} k \sum\limits_{d = 1}^{\left\lfloor \frac{n}{k} \right\rfloor}\mu(d) \sum\limits_{i = 1}^{\left\lfloor \frac{n}{k} \right\rfloor} \sum\limits_{j = 1}^{\left\lfloor \frac{m}{k} \right\rfloor} ij [d | \gcd(i, j)] \\
= & \sum\limits_{k = 1}^{n} k \sum\limits_{d = 1}^{\left\lfloor \frac{n}{k} \right\rfloor}\mu(d) \sum\limits_{i = 1}^{\left\lfloor \frac{n}{kd} \right\rfloor} \sum\limits_{j = 1}^{\left\lfloor \frac{m}{kd} \right\rfloor} ijd^2 [1 | \gcd(i, j)] \\
= & \sum\limits_{k = 1}^{n}k \sum\limits_{d = 1}^{\left\lfloor \frac{n}{k} \right\rfloor}\mu(d) d^2 \sum\limits_{i = 1}^{\left\lfloor \frac{n}{kd} \right\rfloor}i \sum\limits_{j = 1}^{\left\lfloor \frac{m}{kd} \right\rfloor} j
\end{aligned}
$$

我们注意到最后两项实际上都是等差数列求和。我们不妨令 $g(i) = \sum\limits_{i = 1}^{n} i$，那么有：

$$
\begin{aligned}
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \operatorname{lcm}(i, j) = & \sum\limits_{k = 1}^{n}k \sum\limits_{d = 1}^{\left\lfloor \frac{n}{k} \right\rfloor}\mu(d) d^2 g\left( \left\lfloor \frac{n}{kd} \right\rfloor \right) g\left( \left\lfloor \frac{m}{kd} \right\rfloor \right) \\
\end{aligned}
$$

令 $T = kd$，同时我们考虑枚举 $T$，则有：

$$
\begin{aligned}
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{m} \operatorname{lcm}(i, j) = & \sum\limits_{k = 1}^{n}k \sum\limits_{d = 1}^{\left\lfloor \frac{n}{k} \right\rfloor}\mu(d) d^2 g\left( \left\lfloor \frac{n}{T} \right\rfloor \right) g\left( \left\lfloor \frac{m}{T} \right\rfloor \right) \\
= & \sum\limits_{T = 1}^{n} g\left( \left\lfloor \frac{n}{T} \right\rfloor \right) g\left( \left\lfloor \frac{m}{T} \right\rfloor \right) \sum\limits_{d|T}\mu(d)d^2\frac{T}{d} \\
= & \sum\limits_{T = 1}^{n} g\left( \left\lfloor \frac{n}{T} \right\rfloor \right) g\left( \left\lfloor \frac{m}{T} \right\rfloor \right)T \sum\limits_{d|T}d\mu(d)
\end{aligned}
$$

不妨令 $F(n) = \sum\limits_{d|n} d\mu(d)$ 并对 $F(n)$ 进行研究。

我们发现，当 $a \perp b$ 时（当 $a, b$ 互质时）：

$$
\begin{aligned}
F(a) = & \sum\limits_{d|a} d\mu(d) \\
F(b) = & \sum\limits_{d|b} d\mu(d) \\
\end{aligned}
$$

我们不难发现，由 $a \perp b$，故 $a$ 和 $b$ 不存在公共因子，因此 $i | a, \ j | b \Leftrightarrow ij|ab$。又由于 $\mu(n)$ 本身又是积性函数，所以有 $\mu(ab) = \mu(a) \cdot \mu(b)$。由此：

$$
\begin{aligned}
F(a) \cdot F(b) = & \sum\limits_{i|a}i\mu(i)\sum\limits_{j|b}j\mu(j) \\
= & \sum\limits_{ij|ab}ij \cdot \mu(ij) \\
= & \sum\limits_{k|ab}k\mu(k) \\
= & F(ab)
\end{aligned}
$$

我们发现 $F(n)$ 也是个积性函数！既然是积性函数…… 那么一定可以线性筛：

1. $F(1) = 1$；
2. 若 $a$ 是质数，$F(a) = 1 - a$；
3. 若 $a \perp b$，$F(ab) = F(a) \cdot F(b)$；

由此一来，我们就可以 $\mathcal{O}(N)$ 初始化（线性筛 $F(n)$），并借助整除分块 $\mathcal{O}(\sqrt{N})$ 解决询问了。

# 实现

[完整参考代码](https://github.com/codgician/ICPC/blob/master/Luogu/P1829/mobius_inversion.cpp)

# %%%

- An-Amazing-Blog - [莫比乌斯反演-让我们从基础开始](https://www.luogu.org/blog/An-Amazing-Blog/mu-bi-wu-si-fan-yan-ji-ge-ji-miao-di-dong-xi)