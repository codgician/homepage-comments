---
title: HDUOJ 3037 - Saving Beans
date: 2018-05-26 20:00:01
tags: 
- ACM-ICPC
- Mathematics
- Combinatorics
- Lucas' Theorem
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

凛冬将至，松鼠正忙于采取豆豆。它们从 $n$ 棵树上采取豆豆，但处于可持续性发展的考虑，它们一共最多采取 $m$ 粒豆豆。假设每棵树上都有无限多的豆豆，请计算采取的方案种数并将结果对质数 $p$ 取模。

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=3037)


# 排列组合

实际上这个问题可以转换为求下述方程解的个数：
$$
x_1 + x_2 + \dots + x_n \leq m \ (x_i \in N)
$$

我们首先考虑等于 $m$ 的时候解的个数，不难考虑到使用**隔板法**。由于每个 $x_i$ 都可以取 $0$，不妨对它们都加上 $1$ 使其取值范围变为 $[1, +\infty)$ ：
$$
\begin{aligned}
x_1 + x_2 + \dots + x_n & = m \\
\Rightarrow (x_1 + 1) + (x_2 + 1) + \dots + (x_n + 1) & = m + n
\end{aligned}
$$
于是我们就可以愉快地使用隔板法啦，其解的种数为：
$$
\binom{m + n - 1}{n - 1}
$$
也就是：
$$
\binom{m + n - 1}{m}
$$

这样一来，我们所要求的答案即：
$$
\binom{n - 1}{0} + \binom{1 + n - 1}{1} + \binom{2 + n - 1}{2} + \dots +\binom{m + n - 1}{m}
$$
我们联想到组合数的递推式：
$$
\binom{n}{m} = \binom{n - 1}{m} + \binom{n - 1}{m - 1}
$$

于是答案可化简为：
$$
\begin{aligned}
& \binom{n - 1}{0} + \binom{1 + n - 1}{1} + \binom{2 + n - 1}{2} + \dots + \binom{m + n - 1}{m}  \\
& = \binom{n}{0} + \binom{n}{1} + \binom{n + 1}{2} + \dots + \binom{m + n - 1}{m} \\
& = \binom{n + 1}{1} + \binom{n + 1}{2} + \dots + \binom{m + n - 1}{m} \\
& = \dots \\
& = \binom{m + n} {m} 
\end{aligned}
$$
具体求解的时候考虑使用 **Lucas 定理**， 即当 $p$ 是质数时，对于任意整数 $1 \le m \le n$ 时有：
$$
\binom{n}{m} \equiv \binom{n \bmod p}{m \bmod p} \cdot \binom{n \left.\right/ p}{m \left.\right/ p} \pmod{p}
$$

同时在计算大组合数取模的时候，可先计算分子 $n!$ 再计算分母 $m!(n - m)!$ 每项的逆元，全部乘起来即可。复杂度 $\mathcal{O}(n\log{n})$。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/3037/combinatorics_lucas.cpp)



