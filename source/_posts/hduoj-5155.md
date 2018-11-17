---
uuid: 276138e2-d853-11e8-92c3-dbb39e79d86e
title: HDUOJ 5155 - Harry And Magic Box
date: 2018-05-31 17:05:01
updated: 2018-05-31 17:05:01
tags: 
  - ACM-ICPC
  - Mathematics
  - Combinatorics
  - Dynamic Programming
  - Inclusion-Exclusion Principle
  - HDUOJ
category: Solutions
---

皂滑弄人啊... 校赛翻车后的第二天就在自己开的专题里面发现了校赛里一道题的 almost 原题 🙈

# 题面

现有一个 $n \times m$ 的宝箱，已知每一行、每一列上都至少有一颗宝石，试求有多少种摆放方案（结果对 $1000000007$ 取模）。

**数据范围**：

$1 \le n, m \le 50$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=5155)

# 动态规划

## 分析

我们不妨记 $dp[i][j]$ 代表对于 $i \times j$ 宝箱摆放方案的种数。

如果前 $j - 1$ 列都是满足题目要求的，那么只需要第 $j$ 列上至少放一颗宝石就可以使得前 $j$ 列满足要求了。在第 $j$ 列上至少放一颗宝石的方案数为：

$$
\binom{i}{1} + \binom{i}{2} + \dots + \binom{i}{i} = 2^{i} - 1
$$

这种情况的可能种数为：

$$
dp[i - 1][j - 1] \cdot (2^i - 1)
$$

如果前 $j - 1$ 列并不满足要求，且我们希望使得前 $j$ 列能够满足要求，那么对于前 $j - 1$ 列中没有放宝石的行我们必须在第 $j$ 列的该行中放上宝石。我们可以枚举前 $j - 1$ 列中全空列的数量 $k$，而对于剩下的空格则可以随意摆放。

这种情况的可能种数为：

$$
dp[i - k][j - 1] \cdot \binom{i}{k} \cdot 2 ^ {i- k}
$$

于是我们便得到了状态转移方程：

$$
dp[i][j] = dp[i - 1][j - 1] \cdot (2^i - 1) + dp[i - k][j - 1] \cdot \binom{i}{k} \cdot 2 ^ {i- k}
$$

## 实现

[完整参考代码](https://github.com/codgician/ICPC/blob/master/HDUOJ/5155/dp_combinatorics.cpp)

# 容斥原理

## 分析

对于 $n \times m$ 的宝箱，首先我们假设每一行都已经至少存在一颗宝石了，现在我们来讨论列的情况。对 $m$ 列我们假设其中有 $i$ 列每行均无宝石。这种情况下的种数为：

$$
\binom{m}{i}
$$

然后对于剩下的 $m - i$ 列，每一列上的任意一行都是可以随便摆放宝石的（但是不能全空），所以种数为：

$$
2^{m - i} - 1
$$

又由于每一列上有 $n$ 行，所以种数为：

$$
(2^{m - i} - 1)^{n}
$$

现在我们希望求得所有列都非空的情况数，由容斥原理：

$$
f(i) = \sum\limits_{i = 0}^{m - 1} (-1)^{i} \cdot (2^{m - i} - 1)^{n}
$$

所以最终的答案即：

$$
\binom{m}{i} \cdot f(i)
= \sum\limits_{i = 0}^{m - 1} (-1)^{i} \cdot \binom{m}{i} \cdot (2^{m - i} - 1)^{n}
$$

## 实现

[完整参考代码](https://github.com/codgician/ICPC/blob/master/HDUOJ/5155/inclusion_exclusion_principle.cpp)
