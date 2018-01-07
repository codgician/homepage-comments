---
title: Codeforces 909C: Python Indentation
date: 2018-01-07 23:22:00
tags: 
- ACM-ICPC
- Dynamic Programming
- Codeforces
category: Solutions
# mathjax: true
---

# 题面

[题目链接](http://codeforces.com/contest/909/problem/C)

大致题意：

Python 中语句块并没有大括号包围，而是靠缩进来定义的。

现在有一种简化版的 Python，只含有两种语句：

-   `s`：Simple statement，即一般的语句。
-   `f`：For statement，即循环语句（相当于 `for`），并且其循环体不可为空。

现在给你一段没有缩进的这种简化版 Python 的代码，请你计算有多少种合法的 Python 程序。

# 动态规划！

我们来看看第 $n$ 行语句。不管第 $n$ 行语句是哪类语句，其可能的缩进实际上完全取决于第 $n - 1$ 行：

-   若第 $n - 1$ 行是 `f` ，第 $n$ 行的缩进只可能比第 $n - 1$ 行多 1；
-   若第 $n - 1$ 行是 `s` ，第 $n$ 行的缩进可以是 0 ~ 第 $n - 1$ 行缩进中的任意一值（即小于等于 $n - 1$ 行的缩进）。

基于此发现，我们可以用 $dp[i][j]$ 表示状态，表示第 $i$ 行缩进为 $j$ 时的方案个数。边界条件即 $dp[0][0] = 1$。

同时，我们也不难推出状态转移方程：

-   若第 $i  - 1$ 行为 `f`，那么第 $i$ 行缩进为 $j$ 的情况种数实际上等于第 $i - 1$ 行缩进为 $j - 1$ 的情况种数：

-   $$
    dp[i][j] = dp[i - 1][j - 1]
    $$

-   若第 $i - 1$ 行为 `s`，那么第 $i$ 行的缩进一定是**小于等于**第 $i - 1$ 行的缩进。反过来看，也就是说第 $i$ 行缩进为 $j$ 的情况种数是第 $i - 1$ 行缩进为 $j$ , $j + 1$ ... 一直到最大可能的缩进值的所有方案数之和（由于只有 `f` 语句可能带来缩进，所以最大缩进值就是之前语句中 `f` 的个数，下面公式中记作 $M$ ）。

-   $$
    dp[i][j] = \Sigma_{k = j}^{M} dp[i - 1][k]
    $$



那么最后我们所要求的总合法方案数就是（同样记最大可能缩进为 $M$）：
$$
\Sigma_{k = 0}^{M} dp[i][k]
$$


# 具体实现？

## 朴素实现

[Submission #33981342](http://codeforces.com/contest/909/submission/33981342)

算法复杂度达到了 $\mathcal{O}(n^3)$，不 TLE 才怪……

## 树状数组维护前缀和

挖坑待填……

%%%Leohh大佬 [原链接](http://www.cnblogs.com/Leohh/p/8135525.html)

## 倒着循环维护后缀和

[Submission #33983065](http://codeforces.com/contest/909/submission/33983065)

刚才的朴素算法主要是在处理第二种情况（第 $i - 1$ 行为 `s` ）时复杂度高了，其原因则是因为求和的过程中做了很多重复运算。我们发现：
$$
\begin{aligned}
& dp[i][0] = dp[i - 1][0] + dp[i - 1][1] + ... + dp[i - 1][M] \\
& dp[i][1] = dp[i - 1][1] + dp[i - 1][2] + ... + dp[i - 1][M] \\
& ... \\
& dp[i][M] = dp[i - 1][M]
\end{aligned}
$$
因此我们没有必要在计算每一个 $dp[i][j]$ 时都把 $\Sigma_{k = j}^{M} dp[i - 1][k]$ 算一遍，只需要维护一个后缀和就可以了。

```cpp
int suffixSum = 0;
for (int j = maxIndentation; j >= 0; j--)
{
    suffixSum += dp[i - 1][j];
    suffixSum %= mod;
    dp[i][j] = suffixSum;
}
```

## 进一步优化

还没看懂 cdx 大佬的代码，挖坑待填……



# %%%

-   Nickolas - [Codeforces Round #455 (Div.2) Editorial](http://codeforces.com/blog/entry/56666)
-   Leohh - [Codeforces 909C Python Indentation：树状数组优化dp](http://www.cnblogs.com/Leohh/p/8135525.html)

