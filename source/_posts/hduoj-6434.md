---
uuid: ad4a307a-d854-11e8-b9a3-9fd64175cb05
title: HDUOJ 6434 - Count
date: 2018-08-23 13:53:46
updated: 2018-08-23 13:53:46
tags: 
  - Competitive Programming
  - Mathematics
  - Number Theory
  - HDUOJ
category: Solutions
---

# 题面

本题有 $T$ 个询问，对于每个询问 $n$，你需要计算：

$$
\sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{i - 1} \left[ \gcd(i + j, i - j) = 1 \right]
$$

**数据范围**：

$1 \le T \le 10^5$

$1 \le n \le 2 \times 10^7$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6434)

# 分析

题意就是要求满足 $1 \le j < i \le n$ 且 $\gcd(i + j, i - j) = 1$ 的 $(i, j)$ 对个数。

首先 $\gcd(i + j, i - j)$ 并不直观，我们考虑令 $k = i - j$，则有 $k \in [1, i - 1]$，以及：

$$
\begin{aligned}
& \sum\limits_{i = 1}^{n} \sum\limits_{j = 1}^{i - 1} \left[ \gcd(i + j, i - j) = 1 \right] \\
& = \sum\limits_{i = 1}^{n} \sum\limits_{k = 1}^{i - 1} \left[ \gcd(2i - k, k) = 1 \right] \\
& = \sum\limits_{i = 1}^{n} \sum\limits_{k = 1}^{i - 1} \left[ \gcd(2i, k) = 1 \right] \\
\end{aligned}
$$

首先当 $k$ 是偶数时显然 $\gcd(2i, k) \neq 1$，故 $k$ 只可能是奇数。由此，$\gcd(2i, k) = 1$ 与 $\gcd(i, k) = 1$ 等价。

我们知道欧拉函数 $\varphi(i)$ 即表示 $[1, i)$ 中与 $i$ 互质的自然数的个数，由此：

- 若 $i$ 是奇数，我们需要去除 $\varphi(i)$ 中包含的偶数，则答案为 $\frac{1}{2} \cdot \varphi(i)$（不严谨解释见后文）；
- 若 $i$ 是偶数，则 $\varphi(i)$ 中不可能包含偶数，则答案即 $\varphi(i)$。

即：

$$
\sum\limits_{k = 1}^{i - 1} \left[ \gcd(i, k) = 1 \right] = \frac{\varphi(i)}{1 + (i \bmod 2)}
$$

我们记：

$$
g(i) = \frac{\varphi(i)}{1 + (i \bmod 2)}
$$

则原式即：

$$
\sum\limits_{i = 1}^{n} g(i)
$$

预处理出 $g(i)$ 的前缀和并 $\mathcal{O}(1)$ 回答每个询问即可。

---

顺便不严谨地阐述一下为什么 $i$ 是奇数时答案为 $\frac{1}{2} \cdot \varphi(i)$。

首先我们知道 $[1, i -1]$ 显然时奇偶各占一半的。要证 $[1, i - 1]$ 中与 $i$ 互质的数奇偶各占一半，只需要证明 $[1, i - 1]$ 中与 $i$ 不互质的数中就各占一半。

考虑将 $i$ 表示为：

$$
i = p_1^{k_1} \cdot p_2^{k_2} \cdots p_n^{k_n}
$$

其中 $p_i \neq 2$。

由此一来，我们可以将所有小于 $i$ 且与 $i$ 不互质的数表示为：

$$
\begin{aligned}
& p_1, \ 2p_1, \ \dots, \ (p_2^{k_2}p_3^{k_3} \cdots p_n^{k_n} - 1)p_1 \\
& p_2, \ 2p_2, \ \dots, \ (p_1^{k_1}p_3^{k_3} \cdots p_n^{k_n} - 1)p_2 \\
& \dots \\
& p_n, \ 2p_n, \ \dots, \ (p_1^{k_1}p_2^{k_2} \cdots p_{n-1}^{k_{n-1}} - 1)p_n
\end{aligned}
$$

首先对于每一行都是奇数偶数各占一半的。当然上面列举的数中显然是有重复的。若把上面 $n$ 行看作 $n$ 个集合，那么任意 $n$ 个集合并起来后得到的集合依然是奇偶各占一半。那么全集显然也是奇偶各占一半。

# 实现

借助线性筛，我们可以在 $\mathcal{O}(N)$ 的复杂度内初始化欧拉函数，同时我们也可以在 $O(N)$ 时间内初始化 $g(n)$ 的前缀和。初始化好后再对不同询问进行 $\mathcal{O}(1)$ 回答即可。

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/HDUOJ/6434/euler's_totient_function.cpp)
