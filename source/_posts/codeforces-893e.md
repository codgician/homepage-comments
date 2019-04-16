---
uuid: 35f5c390-d84f-11e8-84d5-8b9f56e4c4a9
title: Codeforces 893E - Counting Arrays
date: 2018-07-14 00:30:00
updated: 2018-07-14 00:30:00
tags: 
  - Competitive Programming
  - Combinatorics
  - Codeforces
category: Solutions
---

# 题面

对于正整数 $x$ 和 $y$，求有多少种长度为 $y$ 的序列，其元素的乘积为 $x$。序列中允许出现负数，对于两个序列 $a$ 和 $b$ 只要存在 $a_i \neq b_i$ 就视作两个不同序列。答案对 $10^9 + 7$ 取模。

**数据范围**：

一组数据有 $q$ 次询问，$1 \le q \le 10^5$

$1 \le x,y \le 10^6$

[题目链接](http://codeforces.com/contest/893/problem/E)

# 组合数学

## 思路

首先，我们对 $x$ 分解质因数：
$$
x = p_1^{k_1} \cdot p_2^{k_2} \cdot \dots \cdot p_n^{k_n}
$$

同时，我们知道序列中的任意一个值 $a_i$ 一定可以表示为如下形式：
$$
a_i  = p_1^{t_1} \cdot p_2^{t_2} \cdot \dots \cdot p_n^{t_n}
$$
现在我们先暂且不考虑负数的情况。我们把质因子视作“小球”，序列中的每一个元素视为“箱子”。由此我们可以把问题转换为经典的“球放箱子”问题：现有 $n$ 类小球，第 $i$ 类小球有 $k_i$ 个，同时有 $y$ 个箱子，箱子可以为空，试问把所有小球放到箱子里有多少种放法。

对于第 $i$ 类小球，一共有 $k_i$ 个，同时一个箱子可以放多个。这等效于一共有 $y + k_i - 1$ 个箱子且每个箱子最多放一个小球。因此摆放种数为：
$$
\binom{y + k_i - 1}{k_i}
$$

根据乘法原理，所有小球的总摆放种数为：
$$
\prod_{i = 1}^{n} \binom{y + k_i - 1}{k_i}
$$

接下来，我们来考虑负数。显然，在序列中，$y - 1$ 个数的正负可以是任意的，而乘积的正负可以由剩下的那一个数来决定。所以，总拜访种数为：
$$
2^{y - 1} \cdot \prod_{i = 1}^{n} \binom{y + k_i - 1}{k_i}
$$

## 实现

这道题思路是可以说是非常简单，但是看看数据范围就知道在实现上需要运用一些技巧。

首先，在对 $x$ 分解质因数前需要先线性筛素数，分解质因数时应尽量“剪枝”。

在求组合数时复杂度应做到 $\mathcal{O}(1)$，因此需要预处理 $n!$ 及其对于 $10^9 + 7$ 逆元。

最后，还要预处理 $2$ 的幂。

另外在做这道题的过程中顺便学到了一种[线性预处理逆元的算法](http://blog.miskcoo.com/2014/09/linear-find-all-invert)，但这道题用不到。为了试试这种算法，第一份参考代码中使用了这一算法来预处理逆元。

[完整参考代码 - 1](https://github.com/codgician/Competitive-Programming/blob/master/Codeforces/893E/combinatorics.cpp)

[完整参考代码 - 2](https://github.com/codgician/Competitive-Programming/blob/master/Codeforces/893E/combinatorics_alt.cpp)

# %%%

- miskcoo - [\[数论\] 线性求所有逆元的方法](http://blog.miskcoo.com/2014/09/linear-find-all-invert)
