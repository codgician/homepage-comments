---
uuid: aeadb478-d84f-11e8-a5ca-b39482a2bcb8
title: "Codeforces 1000D - Yet Another Problem on Subsequence"
date: 2018-07-10 18:17:00
updated: 2018-07-10 18:17:00
tags: 
  - ACM-ICPC
  - Dynamic Programming
  - Codeforces
category: Solutions
---

# 题面

如果一个由整数组成的数组 $a_1, a_2, \dots, a_k$，满足 $a_1 = k - 1$ 并且 $a_1 > 0$，我们称其为*好数组*。而如果一个由整数组成的序列可以被划分为多个*好数组*，我们称这个序列是*好序列*。现在给你一个序列，试问有多少个*好子序列*，结果对 998244353 取余。

[题目链接](http://codeforces.com/problemset/problem/1000/D)

# 动态规划！

首先我们不难观察到，好序列实际上就是由若干个好数组拼接得到的。

我们记 $dp[i]$ 代表对于后缀序列 $a_i, a_{i + 1}, \dots, a_k$ 的前缀序列（也就是所有以 $a_i$ 开头的子序列）的答案之和。

如果 $a[i] \le 0$，显然 $dp[i] = 0$。

否则，我们枚举可以拼接下一个好序列的位置 $j$。位置 $j$ 开头的好数组有 $dp[j]$ 种。而在 $i$ 和 $j$ 之间除去已被确定的第 $i$ 位和第 $j$ 位，还有 $j - i - 1$ 位尚未被确定。根据好数组的定义，我们知道从 $i$ 开始的好数组的长度为 $a[i] + 1$，除去已被确定的开头，还有 $a[i]$ 位尚未被确定。由于子序列并不要求连续，所以我们在这 $j - i - 1$ 位种取出 $a[i]$ 位共有 $\binom{j - i - 1}{a[i]}$ 种取法。另外由此我们不难得知 $j$ 的取值范围即 $i + a[i] + 1\le j \le k$。

根据乘法原理，我们不难写出状态转移方程：
$$
dp[i] = \sum\limits_{j = i + a[i] + 1}^{k} \binom{j - i - 1}{a[i]} \cdot dp[j]
$$

显然，在具体实现的时候循环要倒着写。

另外，我们需要初始化 $dp[k + 1]$ 为 $1$。这等于是我们对于每一个序列虚拟了一个第 $k + 1$ 位，以应对整个序列就是一个好数组的情况（比如样例一）。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Codeforces/1000D/dp.cpp)

# %%%

- BledDest - [Educational Codeforces Round 46 Tutorial](http://codeforces.com/blog/entry/60288)
- deerly_ - [CodeForces - 1000D：Yet Another Problem On a Subsequence （DP+组合数）](https://blog.csdn.net/deerly_/article/details/80963405)