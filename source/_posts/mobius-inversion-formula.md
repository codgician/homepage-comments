---
uuid: c44d8340-ea69-11e8-b673-b37e9c899406
title: 浅谈莫比乌斯反演
date: 2018-11-18 13:07:42
updated: 2018-11-19 16:44:12
tags: 
  - Competitive Programming
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
\sum\limits_{d \mid n} \mu(d) =
\begin{cases}
1 & n = 1\\
0 & \text{otherwise} \\
\end{cases}
$$

下面给出简要证明：

1. $n = 1$ 时，显然有 $\sum\limits_{d \mid n} \mu(d) = 1$；
2. $n \neq 1$ 时，记 $n = p_1^{c_1}p_2^{c_2} \dots p_k^{c_k} \ (c_i > 0)$。对于 $d  \mid  n$，$\mu(d) \neq 0$ 当且仅当 $d$ 中不存在完全平方数因子。显然，具有 $i$ 个质因数的 $d$ 有 $\binom{k}{i}$ 个。因此，$\sum\limits_{d \mid n} \mu(d) = \sum\limits_{i = 0}^{k} (-1)^k\binom{k}{i} = 0$。至于为什么等于 $0$，可以考虑由组合数递推式 $\binom{n}{m} = \binom{n - 1}{m} + \binom{n - 1}{m - 1}$ 得证，这里不再详述。

# 莫比乌斯反演

## 形式 #1

### 内容

定义 $F(n), f(n)$ 均为非负整数集合上的两个函数，则有：

$$
F(n) = \sum\limits_{d \mid n}f(d) \Leftrightarrow f(n) = \sum\limits_{d \mid n} \mu(d)F(\frac{n}{d})
$$

简而言之，利用莫比乌斯反演，只要我们知道 $F(n)$ 或是 $f(n)$ 中一者的定义，就可以推知另一者的具体定义。

### 证明

下面我们给出莫比乌斯反演的一种简要证明：

$$
\begin{aligned}
\sum\limits_{d \mid n} \mu(d)F(\frac{n}{d}) = & \sum\limits_{d \mid n} \mu(d) \sum\limits_{k \mid \frac{n}{d}} f(k) \\
= & \sum\limits_{k \mid n} f(k) \sum\limits_{d \mid \frac{n}{k}} \mu(d) \\
\end{aligned}
$$

由前文提到的性质，当且仅当 $\frac{n}{k} = 1$ 时 $\sum\limits_{d \mid \frac{n}{k}} \mu(d) = 1$；否则该式值为 $0$。所以：

$$
\begin{aligned}
\sum\limits_{d \mid n} \mu(d)F(\frac{n}{d}) = f(k)
\end{aligned}
$$

得证。

## 形式 #2

### 内容

定义 $F(n), f(n)$ 均为非负整数集合上的两个函数，则有：

$$
F(n) = \sum\limits_{n \mid d}f(d) \Leftrightarrow f(n) = \sum\limits_{n \mid d} \mu(\frac{d}{n})F(d)
$$

注意这一形式与上一形式最大的不同点，即求和条件由 $d \mid n$ 变为 $n \mid d$。

### 证明

令 $t = \frac{d}{n}$，有：

$$
\begin{aligned}
\sum\limits_{n \mid d} \mu(\frac{d}{n})F(d) = & \sum\limits_{t = 1}^{+\infty}\mu(t)F(nt) \\
= & \sum\limits_{t = 1}^{+\infty}\mu(t)\sum\limits_{nt \mid k}f(k) \\
= & \sum\limits_{n \mid k}f(k)\sum\limits_{t \mid \frac{k}{n}}\mu(t)
\end{aligned}
$$

由前文提到的性质，当且仅当 $\frac{k}{n} = 1$ 时 $\sum\limits_{t \mid \frac{k}{n}}\mu(t) = 1$，否则该式值为 $0$。所以：

$$
\sum\limits_{n \mid d} \mu(\frac{d}{n})F(d) = f(n)
$$

# 应用

## 与欧拉函数联系

这里我们尝试应用一下莫比乌斯反演来推导莫比乌斯函数 $\mu(n)$ 与欧拉函数 $\varphi(n)$ 间的关系。

首先欧拉函数有一个性质：

$$
\sum\limits_{d \mid n} \varphi(d) = n
$$

那么我们不妨令 $F(n) = \sum\limits_{d  \mid  n} \varphi(d) = n$，运用莫比乌斯反演可得：

$$
\begin{aligned}
\varphi(n) = & \sum\limits_{d \mid n} \mu(d)F(\frac{n}{d}) \\
= & \sum\limits_{d \mid n} \mu(d) \cdot \frac{n}{d}
\end{aligned}
$$

整理后可得：

$$
\sum\limits_{d \mid n} \frac{\mu(d)}{d} = \frac{\varphi(n)}{n}
$$

## 与最大公约数联系

给定 $n, m$，求满足 $1 \le i \le n, \ 1 \le j \le m, \ \gcd(i, j) = 1$ 的数对 $\langle i, j \rangle$ 个数（其中 $\langle a, b \rangle$ 和 $\langle b, a \rangle$ 算两个不同的数对）。

换言之，即求：

$$
\sum\limits_{i = 1}^{n}\sum\limits_{j = 1}^{m} [\gcd(i, j) = 1]
$$

暴力的做法显然复杂度是 $\mathcal{O}(n^2)$ 级别的。

我们可以考虑利用莫比乌斯函数的性质：

$$
\begin{aligned}
\sum\limits_{i = 1}^{n}\sum\limits_{j = 1}^{m} [\gcd(i, j) = 1] = & \sum\limits_{i = 1}^{n}\sum\limits_{j = 1}^{m}\sum\limits_{d \mid \gcd(i, j)}\mu(d) \\
= & \sum\limits_{d = 1}^{\min(n, m)}\mu(d) \sum\limits_{i = 1}^{n}\sum\limits_{j = 1}^{m} [d  \mid  \gcd(i, j)] \\
= & \sum\limits_{d = 1}^{\min(n, m)} \mu(d)\left\lfloor \frac{n}{d} \right\rfloor \left\lfloor \frac{m}{d} \right\rfloor
\end{aligned}
$$

我们可以用 $\mathcal{O}(N)$ 的复杂度预处理出 $\mu(n)$ 的前缀和，面对询问时我们可以用 $\mathcal{O}(\sqrt{N})$ 的复杂度进行整除分块并进行求解。

---

那么如果我们要求的是 $\gcd(i, j) = k$ 怎么办？我们仅需做如下变换：

$$
\sum\limits_{i = 1}^{n}\sum\limits_{j = 1}^{m} [\gcd(i, j) = k] = \sum\limits_{i = 1}^{\left\lfloor \frac{n}{k} \right\rfloor}\sum\limits_{j = 1}^{\left\lfloor \frac{m}{k} \right\rfloor} [\gcd(i, j) = 1]
$$

接下来就跟之前的做法一样咯，所以我们能得到：

$$
Ans = \sum\limits_{d = 1}^{\min(\left\lfloor \frac{n}{k} \right\rfloor, \left\lfloor \frac{m}{k} \right\rfloor)} \mu(d) \left\lfloor \frac{n}{kd} \right\rfloor \left\lfloor \frac{m}{kd} \right\rfloor
$$

---

当然，我们也可以向莫比乌斯反演的的方向进行考虑。

我们不妨记 $f(k)$ 代表满足 $\gcd(i, j) = k$ 的 $\langle i, j \rangle$ 对个数。那么 $F(k) = \sum\limits_{k \mid d}f(d)$ 的意义即为满足
 $k \mid \gcd(i, j)$ 的 $\langle i, j \rangle$ 对数。而求满足 $1 \le i \le n, \ 1 \le j \le m$ 范围内 $k \mid \gcd(i, j)$ 这一条件的对数显然等价于 $1 \le i \le \left\lfloor \frac{n}{k} \right\rfloor, \ 1 \le j \le \left\lfloor \frac{m}{k} \right\rfloor$ 范围内 $1 \mid \gcd(i, j)$ 的对数（显然在此范围内的所有数对都满足这一条件）。由此我们可以很容易得到 $F(k)$ 的具体定义：

 $$
 F(k) = \left\lfloor \frac{n}{k} \right\rfloor \left\lfloor \frac{m}{k} \right\rfloor
 $$

 接下来我们就可以利用莫比乌斯反演直接得到 $f(k)$ 的具体定义了：

 $$
 \begin{aligned}
 f(k) = & \sum\limits_{k \mid d} \mu(\frac{d}{k})F(d) \\
 = & \sum\limits_{k \mid d} \mu(\frac{d}{k}) \left\lfloor \frac{n}{d} \right\rfloor \left\lfloor \frac{m}{d} \right\rfloor \\
 \end{aligned}
 $$

不妨令 $t = \frac{d}{k}$，我们便得到了跟之前一样的结果：

$$
f(k) = \sum\limits_{t = 1}^{\min(\left\lfloor \frac{n}{k} \right\rfloor, \left\lfloor \frac{m}{k} \right\rfloor)} \mu(t) \left\lfloor \frac{n}{tk} \right\rfloor \left\lfloor \frac{m}{tk} \right\rfloor
$$

至于更多的应用，强烈安利这篇博文： [莫比乌斯反演-让我们从基础开始](https://www.luogu.org/blog/An-Amazing-Blog/mu-bi-wu-si-fan-yan-ji-ge-ji-miao-di-dong-xi)。

# %%%

- Ronald L. Graham, Donald E. Knuth, Oren Patashnik - Concrete Mathematics, Second Edition
- peng-ym - [莫比乌斯反演](https://www.cnblogs.com/peng-ym/p/8647856.html)
- outer_form - [莫比乌斯反演定理证明（两种形式）](https://blog.csdn.net/outer_form/article/details/50588307)
- An-Amazing-Blog - [莫比乌斯反演-让我们从基础开始](https://www.luogu.org/blog/An-Amazing-Blog/mu-bi-wu-si-fan-yan-ji-ge-ji-miao-di-dong-xi)