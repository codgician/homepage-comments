---
uuid: de0f6d4c-d84f-11e8-9fd0-27d5181e2c9d
title: Codeforces Gym 101630J - Journey from Petersburg to Moscow
date: 2018-10-09 20:39:12
updated: 2018-10-09 20:39:12
tags: 
  - Competitive Programming
  - Graph Theory
  - Shortest Path
  - Codeforces Gym
category: Solutions
#mathjax: true
---

# 题面

高速公路网连接着 $n$ 座城市且包含 $m$ 条**双向**高速公路，经过第 $i$ 条高速公路需要支付费用 $w_i$。节日期间当地交管部门推出了一项优惠政策：如果某车在该高速公路网内经过高速公路的条数多于 $k$，则该车仅需支付其经过所有高速路中费用前 $k$ 大的高速路的费用，而对于剩下的高速公路则无需支付对应费用。现节日期间某司机想从 $1$ 地开车前往 $n$ 地，试问他所需支付的最小费用。

[题目链接](http://codeforces.com/gym/101630/attachments) （J 题）

# 分析

我们不妨把费用当作边长来考虑。

首先考虑**最终答案路径**经过边小于等于 $k$ 条的情况：此时答案就是原图中的最短路。因为既然边数小于 $k$ 条，自然享受不了题目中的优惠政策。如果这条路径享受不了优惠政策都比所有能享受优惠政策的路径享受优惠政策后的长度还要短，那么它自然就是原图中的最短路了。

接下来我们只考虑最终答案路径经过边大于 $k$ 条的情况。我们假设在某条从 $1$ 到 $n$ 的路径上第 $k$ 大的边权为 $x$，那么在计算这条路径的距离时第 $k$ 大以后的边权就要全部视作 $0$。这等效于，我们事先将这条路径上所有的边权都减去 $x$，若边权减去 $x$ 后变为负数（即说明改变边权不属于前 $k$ 大）则将边权赋值为 $0$，然后计算该条路径上所有边权和后再加上 $k \cdot x$。

在这一基础上，我们可以考虑枚举答案路径中第 $k$ 大边权的值。即，对于每一种边权 $c$，都在原图中将边 $e$ 的边权 $w(e)$ 改为 $\max{(0, w(e) - x)}$。接着我们在原图中跑一边单源最短路，记 $1$ 到 $n$ 点的最短路为 $dis(n)$，则可得到将边权 $c$ 当作第 $k$ 长边的答案 $dis(n) + k \cdot c$。取所有的这些值以及原始图中的最短路长度中最小值即为答案。

---

更正式地说，我们记原图为 $G$，记 $G_x$代表与 $G$ 包含边相同，但是对于边 $e \in G$ 的边权 $w(e)$ 在 $G_x$ 中变为 $w_x(e) = \max{(0, w(e) - x)}$。记 $d(x)$ 代表 $G_x$ 中 $1$ 到 $n$ 的最短路长度，记 $f(x) = d(x) + x \cdot k$。我们认为答案，也就是所有路径中前 $k$ 大边权值和的最小值，就是 $ans = \min\limits_{x \ge 0}f(x)$。

下面给出简要证明（主要翻译自官方题解，链接附于文末）：

首先我们证明对任意 $x \ge 0$ 都有 $ans \ge f(x)$。考虑某条路径 $p$，令其所经过的所有边权从大到小为 $c_1 \ge c_2 \ge \dots \ge c_l$。若 $l \le k$ 显然该命题成立。在对于 $l > k$ 时，我们记 $d_p(x)$ 代表在路径 $p$ 上 $1$ 到 $n$ 的距离（也就是该路径的长度），并考虑 $x$ 从 $0$ 增大到 $+\infty$ 过程中 $f_p(x) = d_p(x) + x \cdot k$ 值的变化：

- 若 $x < c_l$，对 $x$ 加 $1$ 将使得 $f_p(x)$ 的值增加：
  $$
  \begin{aligned}
  f_p(x + 1) - f_p(x) & = \sum\limits_{i = 1}^{l}(c_i - x - 1) + k(x + 1) - \sum\limits_{i = 1}^{l}(c_i - x) - kx \\
  & = k - l < 0
  \end{aligned}
  $$

- 若 $c_{i + 1} \le x < c_i$ 且 $i \ge k + 1$，对 $x$ 加 $1$ 将使得 $f_p(x)$ 的值增加：

  $$
  \begin{aligned}
  f_p(x + 1) - f_p(x) & = \sum\limits_{j = 1}^{i}(c_j - x - 1) + k(x + 1) - \sum\limits_{j = 1}^{i}(c_j - x) - kx \\
  & = k - i < 0
  \end{aligned}
  $$

- 若 $c_{k + 1} \le x < c_k$，也就是 $k = i$，有 $f_p(x + 1) - f_p(x) = k - i = 0$。与此同时有：

  $$
  \begin{aligned}
  f_p(x) & = \sum\limits_{i = 1}^{l} \max{(0, c_i - x)} + x \cdot k \\
  & = \sum\limits_{i = 1}^{k}(c_i - x) + x \cdot k \\
  & = \sum\limits_{i = 1}^{k} c_i
  \end{aligned}
  $$

  这恰好就是 $p$ 上前 $k$ 大的边权之和。

- 对于 $c_{i + 1} \le x < c_i$ 且 $i < k$，有：$f_p(x + 1) - f_p(x) = k - i > 0$。

换句话说，对于某个固定的路径 $p$，$f_p(x)$ 的值都先随 $x$ 的增大而减小直到 $c_{k + 1}$ 处出现最小值 $\sum\limits_{i = 1}^{k} c_i$，并在 $x \in [c_{k + 1}, c_k]$ 内保持该值，接着又随着 $x$ 的增大而增大。既然 $f(x)$ 是所有 $f_p(x)$ 中的最小值，则 $f(x)$ 的值不可能超过 $ans$，即最终答案的前 $k$ 大边权之和。

接下来我们进一步证明 $ans = f(x)$。我们依然考虑某条路径 $p$，令其所经过的所有边权从大到小为 $c_1 \ge c_2 \ge \dots \ge c_l$。

- 若 $l \le k$，则当 $x = 0$ 时， $f_p(0)$ 也就是当前路径所有边权之和。
- 若 $l > k$，则在上述分析中不难发现 $f_p(c_k)$ 就是当前路径前 $k$ 大边权之和。

既然 $f(x)$ 是上述值中的最小值，那么它自然就是所有路径中前 $k$ 大边权值和的最小值，即我们要求的答案 $ans$。

复杂度：$\mathcal{O}(m^2\log{n})$

# 实现

据说这道题卡了 SPFA，所以最好用堆优化的 Dijkstra。

改变边权可以考虑在进行 Dijkstra 时进行，这样更容易实现。

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/Codeforces-Gym/101630J/dijkstra.cpp)

# %%%

- [Official Tutorial](http://neerc.ifmo.ru/archive/2017/neerc-2017-analysis.pdf)