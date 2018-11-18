---
uuid: c44d8340-ea69-11e8-b673-b37e9c899406
title: 浅谈莫比乌斯反演
date: 2018-11-18 13:07:42
updated: 2018-11-18 15:12:45
tags: 
  - ACM-ICPC
  - Mathematics
  - Number Theory
category: Notes
---

# 简介

~~最近经常演人/被人演，所以是时候学一学反演了。~~

# 莫比乌斯函数

## 定义

对于正整数 $n$，若对其分解质因数有 $n = p_1^{c_1} p_2^{c_2} \dots p_k^{c_k} \ (c_i > 0)$，则：

$$
\mu(n) =
\begin{cases}
1 & n = 1 \\
0 & \exists c_i > 1 \\
(-1)^k & \text{otherwise}
\end{cases}
$$

通俗地来讲，当 $n = 1$ 时，$\mu(n) = 1$；当 $n$ 的因子中存在完全平方数时，$\mu(n) = 0$。对于剩余情况，也就是对 $n$ 质因数分解可得到 $n = p_1p_2 \dots p_k$ 的情况，$\mu(n) = (-1)^k$，即 $\mu(n)$ 的值取决于 $n$ 不同质因数的个数。

## 性质

$$
\sum\limits_{d|n} \mu(d) =
\begin{cases}
1 & n = 1\\
0 & \text{otherwise} \\
\end{cases}
$$

下面给出简要证明：

1. $n = 1$ 时，显然有 $\sum\limits_{d|n} \mu(d) = 1$；
2. $n \neq 1$ 时，记 $n = p_1^{c_1}p_2^{c_2} \dots p_k^{c_k} \ (c_i > 0)$。对于 $d | n$，$\mu(d) \neq 0$ 当且仅当 $d$ 中不存在完全平方数因子。显然，具有 $i$ 个质因数的 $d$ 有 $\binom{k}{i}$ 个。因此，$\sum\limits_{d|n} \mu(d) = \sum\limits_{i = 0}^{k} (-1)^k\binom{k}{i} = 0$。至于为什么等于 $0$，可以考虑由组合数递推式 $\binom{n}{m} = \binom{n - 1}{m} + \binom{n - 1}{m - 1}$ 得证，这里不再详述。

# 莫比乌斯反演

## 形式 #1

### 内容

定义 $F(n), f(n)$ 均为非负整数集合上的两个函数，则有：

$$
F(n) = \sum\limits_{d|n}f(d) \Leftrightarrow f(n) = \sum\limits_{d|n} \mu(d)F(\frac{n}{d})
$$

简而言之，利用莫比乌斯反演，只要我们知道 $F(n)$ 或是 $f(n)$ 中一者的定义，就可以推知另一者的具体定义。

### 证明

下面我们给出莫比乌斯反演的一种简要证明：

$$
\begin{aligned}
\sum\limits_{d|n} \mu(d)F(\frac{n}{d}) = & \sum\limits_{d|n} \mu(d) \sum\limits_{k|\frac{n}{d}} f(k) \\
= & \sum\limits_{k|n} f(k) \sum\limits_{d|\frac{n}{k}} \mu(d) \\
\end{aligned}
$$

由前文提到的性质，当且仅当 $\frac{n}{k} = 1$ 时 $\sum\limits_{d|\frac{n}{k}} \mu(d) = 1$；否则该式值为 $0$。而 $\frac{n}{k} = 1$ 就意味着只有 $n = k$ 这一种情况，所以：

$$
\begin{aligned}
\sum\limits_{d|n} \mu(d)F(\frac{n}{d}) = f(k)
\end{aligned}
$$

得证。

## 引理

在展示另一个形式前，我们先来看一个引理：
$$
\sum\limits_{d|n} \mu(d)F(\frac{n}{d}) = \sum\limits_{d|n} \mu(\frac{n}{d})F(d)
$$

其实这个很好理解（虽然本垃圾也不知道是否严谨）。

在 $n$ 确定的情况下， $\mu(d)F(\frac{n}{d})$ 实际上可以看作一个仅与 $d$ 有关的函数 $g(d)$。

由整除的相关性质，显然有 $d | n \Leftrightarrow \frac{n}{d} | n$。由此我们易得对于任意函数 $g(d)$，有：

$$
\sum\limits_{d|n}g(d) = \sum\limits_{\frac{n}{d}|n} g(d)
$$

我们不妨令 $d = \frac{n}{t}$，将其带入等式左边即可得到：

$$
\begin{aligned}
\sum\limits_{d|n} \mu(d)F(\frac{n}{d}) = & \sum\limits_{\frac{n}{t}|n} \mu(\frac{n}{t})F(t) \\
= & \sum\limits_{t|n} \mu(\frac{n}{t})F(t)
\end{aligned}
$$

把 $t$ 换回 $d$ 就好了……

## 形式 #2

### 内容

定义 $F(n), f(n)$ 均为非负整数集合上的两个函数，则有：

$$
F(n) = \sum\limits_{n|d}f(d) \Leftrightarrow f(n) = \sum\limits_{n|d} \mu(\frac{d}{n})F(d)
$$

注意这一形式与上一形式最大的不同点，即求和条件由 $d|n$ 变为 $n|d$。

### 证明

由之前提到的引理：

$$
\begin{aligned}
\sum\limits_{n|d} \mu(\frac{d}{n})F(d) = & \sum\limits_{n|d}\mu(d)F(\frac{d}{n}) \\
= & \sum\limits_{n|d}\mu(d)\sum\limits_{\frac{d}{n}|k}f(k) \\
= & \sum\limits_{n|k}f(k)\sum\limits_{\frac{d}{n}|k} \mu(d) \\
= & \sum\limits_{n|k}f(k)\sum\limits_{d|kn} \mu(d) \\
\end{aligned}
$$

由前文提到的性质，当且仅当 $kn = 1$ 时 $\sum\limits_{d|kn} \mu(d) = 1$，否则该式值为 $0$。而 $kn = 1$ 意味着只有 $k = n = 1$ 这一种情况，所以：

$$
\sum\limits_{n|d} \mu(\frac{d}{n})F(d) = f(n)
$$

## 应用

### 与欧拉函数联系

这里我们尝试应用一下莫比乌斯反演来推导莫比乌斯函数 $\mu(n)$ 与欧拉函数 $\varphi(n)$ 间的关系。

首先欧拉函数有一个性质：

$$
\sum\limits_{d|n} \varphi(d) = n
$$

那么我们不妨令 $F(n) = \sum\limits_{d | n} \varphi(d) = n$，运用莫比乌斯反演可得：

$$
\begin{aligned}
\varphi(n) = & \sum\limits_{d|n} \mu(d)F(\frac{n}{d}) \\
= & \sum\limits_{d|n} \mu(d) \cdot \frac{n}{d}
\end{aligned}
$$

整理后可得：

$$
\sum\limits_{d|n} \frac{\mu(d)}{d} = \frac{\varphi(n)}{n}
$$

### 与最大公约数联系

**挖坑待填……**

# %%%

- Ronald L. Graham, Donald E. Knuth, Oren Patashnik - Concrete Mathematics, Second Edition
- peng-ym - [莫比乌斯反演](https://www.cnblogs.com/peng-ym/p/8647856.html)
- outer_form - [莫比乌斯反演定理证明（两种形式）](https://blog.csdn.net/outer_form/article/details/50588307)
- An-Amazing-Blog - [莫比乌斯反演-让我们从基础开始](https://www.luogu.org/blog/An-Amazing-Blog/mu-bi-wu-si-fan-yan-ji-ge-ji-miao-di-dong-xi)