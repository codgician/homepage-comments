---
uuid: 7f324be0-1eb4-11e9-b72f-23c90682c600
title: "Codeforces 1096F - Inversion Expectation"
date: 2019-01-23 10:13:38
updated: 2019-01-23 10:13:38
tags:
  - Competitive Programming
  - Mathematics
  - Probability
  - Data Structure
  - Binary Indexed Tree
  - Codeforces
category: Solutions
---

# 题面

给定一个长为 $n$ 的数列，且该数列中 $1 \sim n$ 都只出现一次。数列中有部分位置的数是未知的，用 $-1$ 表示。试求该数列的期望逆序对对数。答案可以表示为 $\frac{P}{Q}$，输出 $P \cdot Q^{-1} \pmod {998244353}$。

**数据范围**：

$1 \le n \le 2 \times 10^5$

[题目链接](http://codeforces.com/contest/1096/problem/F)

# 分析

我们记未知的数有 $u$ 个。显然，对于给定的数列，所有可能的排列有 $u!$ 种。

显然考虑每一种可能下数组逆序对的对数从而计算期望是十分困难的。因此我们考虑计算每类二元组对总体期望的贡献从而计算期望。我们可以把二元组划分成三大类：

- 已知数和已知数；
- 已知数和未知数；
- 未知数和未知数。

## 已知数和已知数的贡献

由于两者都是已知的，所以期望就是实际的逆序对对数。我们不妨考虑把原数列中所有的未知数全部剔除，那么剩余数组的逆序对个数就是所有已知数和已知数之间对于逆序对数期望的贡献。对于这一部分的贡献，我们可以考虑使用树状数组以 $\mathcal{O}(n\log{n})$ 的复杂度求解。

## 已知数和未知数的贡献

对于这一种情况，我们需要分两种互相类似子情况求解：

### 未知数在前，已知数在后

对于每一个已知数（假设当前已知数的下标为 $i$，值为 $a_i$），我们首先计算其左边未知数的个数 $\text{lft}(i)$，以及大于 $a_i$ 且在数列中尚未出现的数的个数 $\text{gtr}(i)$。对于 $\text{gtr(i)}$ 中的数，显然只有当它们出现在 $a_i$ 左边时才能对期望产生贡献。每个位置上出现 $\text{gtr}(i)$ 中的数的概率为  $\frac{\text{gtr}(i)}{u}$，而 $a_i$ 左边未知数的位置又有 $\text{lft}(i)$ 个，因此，对于第 $i$ 位上的已知数，与其左边的未知数构成逆序对所产生的逆序对数期望为：
$$
\text{lft}(i) \cdot \frac{\text{gtr}(i)}{u}
$$
那么总的贡献即：
$$
\sum\limits_{a_i \neq -1} \left( \text{lft}(i) \cdot \frac{\text{gtr}(i)}{u} \right)
$$

### 已知数在前，未知数在后

与上面一种情况类似，我们需要计算其右边未知数的个数 $\text{rgt}(i)$，以及小于 $a_i$ 且在数列中尚未出现的数 $\text{les}(i)$。对于 $\text{les}(i)$ 中的书只有当出现于 $a_i$ 右边时才能对期望产生贡献，因此第 $i$ 位上的已知数，与其右边的未知数构成逆序对所产生的逆序对数期望为：
$$
\text{rgt}(i) \cdot \frac{\text{les}(i)}{u}
$$
那么总的贡献即：
$$
\sum\limits_{a_i \neq -1} \left( \text{rgt}(i) \cdot \frac{\text{les}(i)}{u} \right)
$$

## 未知数和未知数的贡献

我们不妨考虑把所有已知数从原数列中剔除，于是我们得到一个长度为 $u$ 的完全未知的数列。数列中存在 $\binom{u}{2}$ 对二元组，而每一对二元组时逆序对的概率为 $\frac{1}{2}$。那么序列中逆序对个数的期望即：
$$
\frac{1}{2} \binom{u}{2} = \frac{1}{4} u(u - 1)
$$
将上述三类二元组对逆序对数期望的贡献加起来就是最终的期望。

# 实现

我们可以考虑先算未知数与未知数的贡献，因为其期望只与未知数个数 $u$ 有关。

接下来我们利用树状数组计算已知数与已知数的贡献。先计算它的原因是接下来在计算已知数和未知数的贡献时，我们可以借用树状数组来快速计算 $\text{les}(i)$ 和 $\text{gtr}(i)$。

至于 $\text{lft}(i)$，我们可以在读入时遇到 $-1$ 就记 $\text{lft}(i) = 1$，否则 $\text{lft}(i) = 0$，接下来再对 $\text{lft}$ 从左到右求前缀和得到。

对于 $\text{les}(i)$ 的计算，我们知道不大于 $a_i$ 的数有 $a_i$ 个，而已出现的且小于 $a_i$ 的数就是树状数组 $a_i$ 位的前缀和，即 $\text{query}(a_i)$。那么很显然 $les(i) = a_i - \text{query}(a_i)$，而 $\text{gtr}(i)$ 可以由 $u - les(i)$ 得到。

[完整参考代码](https://github.com/codgician/ICPC/blob/master/Codeforces/1096F/probability_binary_indexed_tree.cpp)

# %%%

- PikMike - [Educational Codeforces Round 57 Editorial](http://codeforces.com/blog/entry/64156)
- xht37 - [CF1096F Inversion Expectation](https://www.cnblogs.com/xht37/p/10198314.html)