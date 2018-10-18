---
title: 浅谈类 Euclid 算法
date: 2018-10-18 22:54:39
tags: 
- ACM-ICPC
- Mathematics
- Number Theory
category: Notes
#mathjax: true
---

> 本文数学公式较长，建议在屏幕较大的设备上阅读。

# 简介

类 Euclid 算法即使用与 Euclid 算法类似的思想解决一类求和式的计算问题。可用类 Euclid 算法解决的求和式主要有以下三种：

$$
\begin{aligned}
f(a, b, c, n) & = \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor} \\
g(a, b, c, n) & = \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{ai + b}{c} \right\rfloor} \\
h(a, b, c, n) & = \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor}^2 \\
\end{aligned}
$$

后文中将简称它们为 $f$ 式，$g$ 式与 $h$ 式。

# 引理

## #1

$$
a \le \left\lfloor \frac{c}{b} \right\rfloor \Leftrightarrow ab \le c
$$

证明比较显然，这里就略去了。

## #2

$$
a < \left\lfloor \frac{c}{b} \right\rfloor \Leftrightarrow ab < c - b + 1
$$

下面给出简略证明过程：

$$
\begin{aligned}
a < \left\lfloor \frac{c}{b} \right\rfloor
& \Leftrightarrow a \le \left\lfloor \frac{c}{b} \right\rfloor - 1 \\
& \Leftrightarrow ab \le c - b \\
& \Leftrightarrow ab < c - b + 1 \\
\end{aligned}
$$

## #3

$$
\begin{aligned}
n^2
= & 2 \cdot \frac{1}{2}(n - 1)n + n \\
= & 2 \sum\limits_{i = 0}^{n - 1} {i} + n \\
\end{aligned}
$$

在后面的推导中可能会不加证明地使用上述三个结论。

# f 式

## 形式

$$
f(a, b, c, n) = \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor}
$$

## 推导

首先我们考虑求和式中每一项都大于 $0$ 的情况，即当 $a \ge c$ 或 $b \ge c$ 时：

$$
\begin{aligned}
f(a, b, c, n)
& = \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor} \\
& = \sum\limits_{i = 0}^{n} {\left[ \left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor + \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor \right]} \\
& = \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{a}{c} \right\rfloor} + \sum\limits_{i = 0}^{n} {\left\lfloor \frac{b}{c} \right\rfloor} + \sum\limits_{i = 0}^{n} {\left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor} \\
& = \left\lfloor \frac{a}{c} \right\rfloor \frac{1}{2}n(n + 1) + \left\lfloor \frac{b}{c} \right\rfloor (n + 1) + f(a \bmod c, b \bmod c, c, n) \\
\end{aligned}
$$

接下来我们考虑存在某些项等于 $0$ 的情况，即当 $a, b < c$ 时：

$$
\begin{aligned}
f(a, b, c, n) 
& = \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor} \\
& = \sum\limits_{i = 0}^{n} \sum\limits_{j = 0}^{\left\lfloor \frac{ai + b}{c} \right\rfloor - 1} {1} \\
& = \sum_{j = 0}^{\left\lfloor \frac{an + b}{c} \right\rfloor - 1} \sum\limits_{i = 0}^{n} {(\left\lfloor \frac{ai + b}{c} \right\rfloor > j)}
\end{aligned}
$$

我们不妨令 $m = \left\lfloor \frac{an + b}{c} \right\rfloor$，则有：

$$
\begin{aligned}
f(a, b, c, n)
& = \sum_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {( \left\lfloor \frac{ai + b}{c} \right\rfloor > j )} \\
& = \sum_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {\left[ cj < (ai + b) - c + 1 \right]} \\
& = \sum\limits_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {( \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor < i )} \\
\end{aligned}
$$

不难发现，$\sum\limits_{i = 0}^{n} {(\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor < i)}$ 即为 $n - \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor$。故有：

$$
\begin{aligned}
f(a, b, c, n)
& = \sum\limits_{j = 0}^{m - 1} {(n - \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor)} \\
& = nm - \sum\limits_{j = 0}^{m - 1} {\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor} \\
& = nm - f(c, c - b - 1, a, m - 1) \\
\end{aligned}
$$

现在假设我们有 $f(a, b, c, n)$，其中 $a \ge c$。经过情况 $1$ 中的一次运算后 $(a, b, c, n)$ 变为 $(a \bmod c, b \bmod c, c, n)$。此时我们再应用情况 $2$ 进行一次运算四个参数就变成了 $(c, c - b \bmod c - 1, a \bmod c, m - 1)$。我们注意观察第 $1$ 个参数和第 $3$ 个参数，经过上述两次运算后由 $(a, c)$ 变成了 $(c, a \bmod c)$。这与 Euclid 算法存在极大的相似之处，因此我们把这种算法称作类 Euclid 算法。至于时间复杂度，显然也是与 Euclid 算法相同的，为 $\mathcal{O}(\log{n})$ 级别。

## 结论

令 $m = \left\lfloor \frac{an + b}{c} \right\rfloor$，有：

$$
f(a, b, c, n) = 
\begin{cases}
\left\lfloor \frac{a}{c} \right\rfloor \frac{n(n + 1)}{2} + \left\lfloor \frac{b}{c} \right\rfloor (n + 1) + f(a \bmod c, b \bmod c, c, n) & a \ge c \lor b \ge c \\
nm - f(c, c - b - 1, a, m - 1) & \text{otherwise} \\
\end{cases}
$$

# g  式

## 形式

$$
g(a, b, c, n) = \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{ai + b}{c} \right\rfloor}
$$

## 推导

若 $a \ge c$ 或 $b \ge c$，有：

$$
\begin{aligned}
g(a, b, c, n) & = \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{ai + b}{c} \right\rfloor} \\
& = \sum\limits_{i = 0}^{n} {i \cdot ( \left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor + \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor)} \\
& = \sum\limits_{i = 0}^{n} {i^2 \left\lfloor \frac{a}{c} \right\rfloor} + \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{b}{c} \right\rfloor} + \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor} \\
& = \left\lfloor \frac{a}{c} \right\rfloor \frac{1}{6}n(n + 1)(2n + 1) + \left\lfloor \frac{b}{c} \right\rfloor \frac{1}{2}n(n + 1) + g(a \bmod c, b \bmod c, c, n) \\
\end{aligned}
$$

若 $a, b < c$，有：

$$
\begin{aligned}
g(a, b, c, n) & = \sum\limits_{i = 0}^{n} {i \left\lfloor \frac{ai + b}{c} \right\rfloor} \\
& = \sum\limits_{i = 0}^{n} \sum\limits_{j = 0}^{\left\lfloor \frac{ai + b}{c} \right\rfloor - 1} {i} \\
& = \sum_{j = 0}^{\left\lfloor \frac{an + b}{c} \right\rfloor - 1} \sum\limits_{i = 0}^{n} {i \cdot ( \left\lfloor \frac{ai + b}{c} \right\rfloor > j)} \\
\end{aligned}
$$

我们依然记 $m = \left\lfloor \frac{an + b}{c} \right\rfloor$，则有：

$$
\begin{aligned}
g(a, b, c, n)
& = \sum_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {i \cdot ( \left\lfloor \frac{ai + b}{c} \right\rfloor > j)} \\
& = \sum_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {i \cdot \left[ cj < (ai + b) - c + 1 \right]} \\
& = \sum_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {i \cdot (\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor < i)} \\
\end{aligned}
$$

不难发现，$\sum\limits_{i = 0}^{n} {i \cdot (\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor < i)}$ 可看作一个首项为 $\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor + 1$，末项为 $n$ 的等差数列求和，即等于 $\frac{1}{2} \cdot(\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor + 1 + n) \cdot (n - \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor)$。故有：

$$
\begin{aligned}
g(a, b, c, n)
& = \sum\limits_{j = 0}^{m - 1} {\frac{1}{2} (\left\lfloor 
\frac{cj + c - b - 1}{a} \right\rfloor + n + 1) \cdot (n - \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor)} \\
& = \frac{1}{2} \sum\limits_{j = 0}^{m - 1} {\left[ n(n + 1) - {\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor}^2 - \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor \right]} \\
& = \frac{1}{2} \left[ \sum\limits_{j = 0}^{m - 1} {n(n + 1)} - \sum\limits_{j = 0}^{m - 1} {\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor}^2 - \sum\limits_{j = 0}^{m - 1} {\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor} \right] \\
& = \frac{1}{2} \left[ mn(n + 1) - h(c, c - b - 1, a, m - 1) - f(c, c - b - 1, a, m - 1) \right] \\
\end{aligned}
$$

看来 $g$ 式最后推出来还会跟 $h$ 式有关系……

## 结论

$$
g(a, b, c, n) = 
\begin{cases}
\left\lfloor \frac{a}{c} \right\rfloor \frac{1}{6}n(n + 1)(2n + 1) + \left\lfloor \frac{b}{c} \right\rfloor \frac{1}{2}n(n + 1) + g(a \bmod c, b \bmod c, c, n) & a \ge c \lor b \ge c \\
\frac{1}{2} \cdot \left[ mn(n + 1) - h(c, c - b - 1, a, m - 1) - f(c, c - b - 1, a, m - 1) \right] & \text{otherwise} \\
\end{cases}
$$

# h 式

## 形式

$$
h(a, b, c, n) = \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor}^2
$$

## 推导

若 $a \ge c$ 或 $b \ge c$，有：

$$
\begin{aligned}
h(a, b, c, n) = & \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor}^2 \\
= & \sum\limits_{i = 0}^{n} {(\left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor + \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor)^2} \\
= & \sum\limits_{i = 0}^{n} {(\left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor)^2} + 2 \sum\limits_{i = 0}^{n} (\left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor) \cdot \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor \\
& + \sum\limits_{i = 0}^{n} {\left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor}^2 \\
\end{aligned}
$$

为了方便展示，我们分开化简这三项：

$$
\begin{aligned}
\sum\limits_{i = 0}^{n} & {(\left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor)^2} \\
= & \left\lfloor \frac{a}{c} \right\rfloor^2 \sum\limits_{i = 0}^{n} {i^2} + 2\left\lfloor \frac{a}{c} \right\rfloor \left\lfloor \frac{b}{c} \right\rfloor \sum\limits_{i = 0}^{n} {i} + {\left\lfloor \frac{b}{c} \right\rfloor}^2 \sum\limits_{i = 0}^{n} {1} \\
= & \left\lfloor \frac{a}{c} \right\rfloor^2 \frac{1}{6}n(n + 1)(2n + 1) + \left\lfloor \frac{a}{c} \right\rfloor \left\lfloor \frac{b}{c} \right\rfloor 2n(n + 1) + \left\lfloor \frac{b}{c} \right\rfloor^2 (n + 1) \\
\\
2\sum\limits_{i = 0}^{n} & {(\left\lfloor \frac{a}{c} \right\rfloor i + \left\lfloor \frac{b}{c} \right\rfloor)} \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor \\
= & 2\left\lfloor \frac{a}{c} \right\rfloor \sum\limits_{i = 0}^{n} {i \cdot \left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor} + 2\left\lfloor \frac{b}{c} \right\rfloor \sum\limits_{i = 0}^{n} {\left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor} \\
= & 2\left\lfloor \frac{a}{c} \right\rfloor g(a \bmod c, b \bmod c, c, n) + 2\left\lfloor \frac{b}{c} \right\rfloor f(a \bmod c, b \bmod c, c, n) \\
\\
\sum\limits_{i = 0}^{n} & {\left\lfloor \frac{(a \bmod c)i + (b \bmod c)}{c} \right\rfloor}^2 \\
= & h(a \bmod c, b \bmod c, c, n) \\
\end{aligned}
$$

加起来并整理后有：

$$
\begin{aligned}
h & (a, b, c, n) \\
= & \left\lfloor \frac{a}{c} \right\rfloor^2 \frac{1}{6}n(n + 1)(2n + 1) + \left\lfloor \frac{a}{c} \right\rfloor \left\lfloor \frac{b}{c} \right\rfloor 2n(n + 1) + \left\lfloor \frac{b}{c} \right\rfloor^2 (n + 1) \\
& + 2\left\lfloor \frac{a}{c} \right\rfloor g(a \bmod c, b \bmod c, c, n) + 2\left\lfloor \frac{b}{c} \right\rfloor f(a \bmod c, b \bmod c, c, n) + h(a \bmod c, b \bmod c, c, n)
\end{aligned}
$$

若 $a, b < c$，有：

$$
\begin{aligned}
h(a, b, c, n) = & \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor}^2 \\
= & \sum\limits_{i = 0}^{n} (2 \sum\limits_{j= 0}^{\left\lfloor \frac{ai + b}{c} \right\rfloor - 1} {j} + \left\lfloor \frac{ai + b}{c} \right\rfloor) \\
= & 2\sum\limits_{j = 0}^{\left\lfloor \frac{an + b}{c} \right\rfloor - 1} \sum\limits_{i = 0}^{n} {j \cdot (j < \left\lfloor \frac{ai + b}{c} \right\rfloor)} + \sum\limits_{i = 0}^{n} {\left\lfloor \frac{ai + b}{c} \right\rfloor} \\
\end{aligned}
$$

我们依然记 $m = \left\lfloor \frac{an + b}{c} \right\rfloor$，则有：

$$
\begin{aligned}
h(a, b, c, n)
= & 2\sum\limits_{j = 0}^{m - 1} \sum\limits_{i = 0}^{n} {j \cdot (j < \left\lfloor \frac{ai + b}{c} \right\rfloor)} + f(a, b, c, n) \\
= & 2\sum\limits_{j = 0}^{m - 1} j \cdot \sum\limits_{i = 0}^{n} {\left[ cj < (ai + b) - c + 1 \right]} + f(a, b, c, n) \\
= & 2\sum\limits_{j = 0}^{m - 1} j \cdot \sum\limits_{i = 0}^{n} {(\left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor < i)} + f(a, b, c, n) \\
= & 2\sum\limits_{j = 0}^{m - 1} j \cdot(n - \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor) + f(a, b, c, n) \\
= & 2n\sum\limits_{j = 0}^{m - 1} {j} - 2\sum\limits_{j = 0}^{m - 1} {j \cdot \left\lfloor \frac{cj + c - b - 1}{a} \right\rfloor} + f(a, b, c, n) \\
= & 2n \cdot \frac{1}{2}(m - 1)m - 2g(c, c - b - 1, a, m - 1) + f(a, b, c, n) \\
\end{aligned}
$$

我们带入之前推 $f$ 式得到的结论可得：

$$
\begin{aligned}
h(a, b, c, n)
= & n(m - 1)m - 2g(c, c - b - 1, a, m - 1) + nm - f(c, c - b - 1, a, m - 1) \\
= & nm^2 - 2g(c, c - b - 1, a, m - 1) - f(c, c - b - 1, a, m - 1) \\
\end{aligned}
$$

## 结论

$$
h(a, b, c, n) =
\begin{cases}
\left\lfloor \frac{a}{c} \right\rfloor^2 \frac{1}{6}n(n + 1)(2n + 1) + \left\lfloor \frac{a}{c} \right\rfloor \left\lfloor \frac{b}{c} \right\rfloor 2n(n + 1) + \left\lfloor \frac{b}{c} \right\rfloor^2 (n + 1) \\
+ 2\left\lfloor \frac{a}{c} \right\rfloor g(a \bmod c, b \bmod c, c, n) + 2\left\lfloor \frac{b}{c} \right\rfloor f(a \bmod c, b \bmod c, c, n) \\
+ h(a \bmod c, b \bmod c, c, n) & a \ge c \lor b \ge c \\
nm^2 - 2g(c, c - b - 1, a, m - 1) - f(c, c - b - 1, a, m - 1) & \text{otherwise} \\
\end{cases}
$$

# 代码实现

咕咕咕……

# %%%

- Bill Yang - [「bsoj4936」「模板题」类欧几里得 - 类欧几里得 附简要学习笔记](https://blog.bill.moe/bsoj4936-euclidean/)
- WerKeyTom_FTD - [类欧几里得算法小结](https://blog.csdn.net/werkeytom_ftd/article/details/53812718)
- Cold_Chair - [类欧几里得算法乱搞记](https://blog.csdn.net/Cold_Chair/article/details/78857161)