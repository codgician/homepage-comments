---
uuid: 04d4b390-d84d-11e8-92fe-f38ce4f3d6bb
title: HDUOJ 6275 - Mod, Xor and Everything
date: 2018-10-25 18:10:23
updated: 2018-10-25 18:10:23
tags: 
  - Competitive Programming
  - Mathematics
  - Number Theory
  - Quasi-Euclidean Algorithm
  - HDUOJ
category: Solutions
#mathjax: true
---

# 题面

对于给定的 $n$，计算：

$$
(n \bmod 1) \oplus (n \bmod 2) \oplus \dots \oplus [n \bmod (n - 1)]
$$

其中 $\oplus$ 表示按位异或。

**数据范围**：

$n \le 10^{12}$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6275)

[题面链接](http://acm.hdu.edu.cn/downloads/CCPC2018-Hangzhou-ProblemSet.pdf)（L 题）

由于 HDUOJ 卡常丧心病狂，建议读者前往 [异或和 - 题目 - LibreOJ](https://loj.ac/problem/6344) 提交（注意输入格式略微存在出入）。

# 分析

对于异或运算，我们不妨依据位运算的位独立性对于其二进制下的每一位逐位考虑。

对于数 $n$，其二进制第 $k$ 位上的值为：

$$
\left\lfloor \frac{n}{2^k} \right\rfloor \mod 2
$$

那么，对于第 $k$ 位，经过题目中异或运算后这一位上的答案为：

$$
\sum\limits_{i = 1}^{n} \left\lfloor \frac{n \bmod i}{2^k} \right\rfloor \mod 2
$$

我们对其做一点小小的变换：

$$
\begin{aligned}
& \sum\limits_{i = 1}^{n} \left\lfloor \frac{n \bmod i}{2^k} \right\rfloor \mod 2 \\
= & \sum\limits_{i = 1}^{n} \left\lfloor \frac{n - \left\lfloor \frac{n}{i} \right\rfloor i}{2^k} \right\rfloor \mod 2
\end{aligned}
$$

我们发现这个式子非常类似于类欧几里德可解决的 $f$ 式子（有一点不一样，但是后文会通过一些变换使其一样）。有关类欧几里德算法的相关内容可参考我的另一篇博客：[浅谈类欧几里德算法](https://blog.codgician.pw/2018/10/18/quasi-euclidean-algorithm/)。

至于 $\left\lfloor \frac{n}{i} \right\rfloor$，我们可以对其进行整除分块。

对于满足 $\left\lfloor \frac{n}{i} \right\rfloor = t$ 的某一块，我们记块的最左端为 $l$，最右端为 $r$，显然有：

$$
\left\lfloor \frac{n}{l} \right\rfloor = \left\lfloor \frac{n}{r} \right\rfloor = t
$$

以及：

$$
\begin{aligned}
n \bmod r & = n - \left\lfloor \frac{n}{r} \right\rfloor r \\
& = n - tr \\
\end{aligned}
$$

那么对于这一块内求和：

$$
\begin{aligned}
\sum\limits_{i = l}^{r} \left\lfloor \frac{-ti + n}{2^k} \right\rfloor = & \sum\limits_{i = -r}^{-l} \left\lfloor \frac{ti + n}{2^k} \right\rfloor \\
= & \sum\limits_{i = 0}^{r - l} \left\lfloor \frac{t(i - r) + n}{2^k} \right\rfloor \\
= & \sum\limits_{i = 0}^{r - l} \left\lfloor \frac{ti + (n - tr)}{2^k} \right\rfloor \\
= & \sum\limits_{i = 0}^{r - l} \left\lfloor \frac{ti + (n \bmod r)}{2^k} \right\rfloor \\
= & f(t, n \bmod r, 2^k, r - l)
\end{aligned}
$$

简而言之，我们首先通过整除分块得到每一块的 $l$ 和 $r$，并求出这一段中每一位对应的结果，最后将所有段的结果求和并将不同位“组装”起来就是最终答案了。

复杂度：$\mathcal{O}(\sqrt{n}\log{n})$。

# 实现

这道题卡常呜呜呜。

在 $n$ 比较小的时候，$\mathcal{O}(n)$ 暴力是比类欧 $\mathcal{O}(\sqrt{n}\log{n})$ 跑得还要快的…… 故考虑设定一个阈值 $lim$，当 $i$ 不大于 $lim$ 的时候跑暴力，大于 $lim$ 的时候跑类欧。对于本弱写的代码，实测得到 $lim$ 大致取值在 $2 \times 10^7 \sim 4 \times 10^7$ 范围内时可以在 HDUOJ 上卡进时限。而在 LibreOJ 上这个范围就要宽很多了。

[完整参考代码](https://github.com/codgician/ICPC/blob/master/HDUOJ/6275/quasi_euclidean.cpp)

# %%%

- rzO_KQP_Orz - [【hdu6275】【2017ccpc杭州L】Mod, Xor and Everything 题解](https://blog.csdn.net/rzO_KQP_Orz/article/details/83120181?utm_source=blogxgwz0)
