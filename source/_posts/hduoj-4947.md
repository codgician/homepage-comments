---
uuid: c0cc1020-efb8-11e8-81be-a1a0820cc907
title: HDUOJ 4947 - GCD Array
date: 2018-11-24 23:52:32
updated: 2018-11-24 23:52:32
tags:
  - ACM-ICPC
  - Mathematics
  - Number Theory
  - Data Structure
  - Binary Indexed Tree
  - HDUOJ
category: Solutions
---

# 题面

对于长度为 $l$ 且初始全为 $0$ 的序列 $a$（下标 $1 \sim l$），有 $Q$ 次操作。操作分为两种：

1. 给定 $n, d, v$，对于每个满足 $\gcd(x, n) = d$ 的 $x$，为 $a_x$ 加上 $v$；
2. 给定 $x$，询问 $\sum\limits_{i = 1}^{x} a_i$。

**数据范围**：

$1 \le l, Q \le 5 \times 10^4$

$1 \le n, d, v \le 2 \times 10^5$

$1 \le x \le l$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4947)

# 分析

我们记 $\Delta{a}(x)$ 代表某次进行第一种操作时位置 $x$ 上值的增量（假设 $d | n$），有：

$$
\begin{aligned}
\Delta{a}(x) = & v \cdot [\gcd(x, n) = d] \\
= & v \cdot [\gcd(\frac{x}{d}, \frac{n}{d}) = 1]\\
\end{aligned}
$$

借助莫比乌斯函数性质，有：

$$
\begin{aligned}
\Delta{a}(x) = & \sum\limits_{k|\gcd(\frac{x}{d}, \frac{n}{d})} \mu(k) \cdot v \\
= & \sum\limits_{k|\frac{x}{d}, k|\frac{n}{d}} \mu(k) \cdot v \\
= & \sum\limits_{k|\frac{n}{d}} \sum\limits_{kd|x} \mu(k) \cdot v \\
\end{aligned}
$$

我们注意到，$\sum\limits_{kd|x} \mu(k) \cdot v$ 看起来十分类似莫比乌斯反演中的 $F(n)$ 函数。既然如此，我们不妨考虑引入一个辅助数组 $f$，使其满足：
$$
a(x) = \sum\limits_{k|x}f(k)
$$
由此一来，我们可以通过维护 $f(x)$ 并通过莫比乌斯反演从而求得 $a(x)$。

根据上面推导而来的式子，我们不难发现，操作一实则是对于 $\frac{n}{d}$ 的每一个因子 $k$，向 $f(kd)$ 中增加 $\mu(k) \cdot v$。

---

而对于操作二：
$$
\begin{aligned}
\sum\limits_{i = 1}^{x} a(i) = & \sum\limits_{i = 1}^{x}\sum\limits_{k|i}f(k) \\
= & \sum\limits_{k = 1}^{x} \left\lfloor \frac{x}{k} \right\rfloor f(k)
\end{aligned}
$$
整除分块搞一下就好了…… 由于分块需要快速求出区间和，所以可以考虑使用树状数组维护 $f(x)$。

---

接下来我们简要分析一下复杂度：

对于操作1，$\frac{n}{d}$ 分解质因数的复杂度 $\mathcal{O}(\sqrt{N})$，加上树状数组单点更新后复杂度 $\mathcal{O}(\sqrt{N}\log{N})$；

对于操作2，整除分块复杂度 $\mathcal{O}(\sqrt{N})$，加上树状数组区间查询后复杂度 $\mathcal{O}(\sqrt{N}\log{N})$。

妙啊！

# 实现

[完整参考代码](https://github.com/codgician/ICPC/blob/master/HDUOJ/4947/mobius_inversion_binary_indexed_tree.cpp)

# %%%

- flipped - 【HDU4947】GCD Array (莫比乌斯反演+树状数组)](https://www.cnblogs.com/flipped/p/HDU4947.html)