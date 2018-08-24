---
title: HDUOJ 6331 - Walking Plan
date: 2018-08-07 21:27:23
tags: 
- ACM-ICPC
- Graph Theory
- Shortest Path
- Partitioning
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

比特城里有 $n$ 个路口，$m$ 条**单向**街道，第 $i$ 条街道的长度为 $w_i$。最近小Q计划坚持长走 $q$ 天：在第 $i$ 天，小Q计划从 $s_i$ 路口出发，通过**至少** $k_i$ 条街道并最终到达 $t_i$ 路口。

虽然小Q看起来很爱运动但其实很懒，所以他希望能使得他每天所走的路程最少。请编写程序计算小Q每一天最少需要走的距离。

**数据范围**：

$T$ 组测试数据，$1 \le T \le 10$

$2 \le n \le 50$

$1 \le m, w_i, k_i \le 10000$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6331)

# 分析

记 $G[i][j]$ 代表一步从 $i$ 到 $j$ 的距离（需要注意的是当 $i = j$ 时 $G[i][j] = +\infty$，因为图中没有自环所以一步是不可能还待在出发点的）。另外，记 $f[t][i][j]$ 代表从 $i$ 出发**恰好**经过 $k$ 条边到达 $j$ 的最短路。显然：
$$
f[t][i][j] = f[t - 1][i][k] + G[k][j]
$$


另外，对于 $f$ 本身，也有：
$$
f[t][i][j] = \min_{1 \le x < t}\{f[x][i][k] + f[t - x][k][j] \}
$$
我们不妨**把这一合并过程看作类似矩阵乘法的过程**，那么就有：
$$
f[t] = G^t
$$
而每执行这样一次乘法的过程复杂度就高达 $\mathcal{O}(n^3)$，如果我们要朴素地预处理出 $f$ 的话复杂度便高达 $\mathcal{O}(kn^3)$，显然是不可接受的。即使我们尝试用矩阵快速幂优化，那么单次查询复杂度便高达 $\mathcal{O}(n^3\log{k})$，乘上 $m$ 后显然也是不可接受的。

于是我们只好求助…… 万能的分块（逃

我们可以取块长 $L = \sqrt{10000} = 100$。记 $A = \lfloor \frac{k}{100} \rfloor$，$B = k \mod 100$。

然后我们把每次询问的过程分成两步：

- 从 $s$ 出发**恰好**经过 $L \cdot A$ 条边到达某个点 $u$；
- 从 $u$ 出发**至少**经过 $B$ 条边到达 $t$。

这样一来我们可以考虑开两个数组 $f_A$ 和 $f_B$。其中 $f_A[t][i][j]$ 表示从 $i$ 到 $j$ **恰好**经过 $L \cdot t$ 条边到达 $j$ 的最短路，而 $f_B[t][i][j]$ 则表示从 $i$ 到 $j$ **至少**经过 $t$ 条边到达 $j$ 的最短路。那么对于每一次询问，其答案实际上就是：
$$
ans = \min \{ f_A[A][s][u] + f_B[B][u][t] \}
$$
只需要枚举 $u$ 就可以了，所以每一次询问我们可以在 $\mathcal{O}(n)$ 的时间复杂度内解决。

接下来谈谈预处理。首先，$f_A$ 可以通过如下方式得到：
$$
f_A[t] = (G^{L})^t
$$
但是对于 $f_B$，由于它表示的是**至少**经过 $t$ 条边，我们还需要另跑一次 Floyd，得到最短路矩阵 $dis[i][j]$（表示从 $i$ 到 $j$ 的最短路），然后我们再把 $G^t$ 和 $dis[i][j]$ 按照之前说的方式合并，就得到了经过至少 $t$ 条边从 $i$ 到 $j$ 的最短路。

预处理复杂度就被压到了 $\mathcal{O}(n^3\sqrt{k})$。这样一来，总的复杂度就是 $\mathcal{O}(n^3\sqrt{k} + qn)$。

# 实现

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/6331/floyd_partitioning.cpp)

# %%%

- Claris - [2018 Multi-University Training Contest 3 solutions BY Claris](http://bestcoder.hdu.edu.cn/blog/2018-multi-university-training-contest-3-solutions-by-claris/)