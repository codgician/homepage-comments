---
title: 第九届“浪潮杯”山东省程序设计竞赛
date: 2018-07-19 15:21:00
tags:
- ACM-ICPC
- Contest
- Solution
category: Solutions
thumbnail: /2018/07/19/ninth-shandong-provincial-contest/cover.jpg
#mathjax: true
---
# 简介

"**2018杭电ACM集训队单人排位赛 - 3**" 采用了这一套题，然后我成功地崩掉了…… 这里补上题解。

我对于最后两道题还暂时存在知识盲区，故这里只提供 $A \sim H$ 前 $8$ 道题的题解。

[重现赛地址](https://www.nowcoder.com/acm/contest/123)

# A: Anagram 

贪心。

对于 **A** 串中的每一个字符，尽量使它变换次数最少，这样可以保证总变换次数最少。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123A/greedy.cpp)

 

# B: Bullet

二分图匹配 + 二分答案。

由每行、每列只能选一个怪物打，很容易想到将到二分图匹配的模型。我们把 $N$ 行和 $N$ 列分别看作 $N$ 个行节点和 $N$ 个列节点，并且分别把它们看作二分图的左部和右部。

首先，我们能很容易地算出最大匹配数。只要 $A_{ij} > 0$（即怪物存在），我们就在左部 $i$ 节点和右部 $j$ 节点间建一条边。接着，我们跑一遍匈牙利匹配就可以算出最大匹配数 $\text{maxAns}$。

接下来，我们来二分答案。记 $A_{ij}$ 中最小的非零经验值为 $L$，最大的经验值为 $R$，那么显然答案区间即 $[L, R]$。对于当前要判断可行性的最大经验值 $mid$，我们只对 $A_{ij} \ge mid$ 的点建边。由于题目要求的是在打怪数量最多前提下的最大经验值，我们只需要跑一遍匈牙利算法看一下在当前图中最大匹配数是否等于 $\text{maxAns}$ 即可。

复杂度：$\mathcal{O}(N^2\log{N})$

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123B/hungarian_binary_search.cpp)

关于匈牙利算法的更多内容可以参考我的另一篇博文：[浅谈匈牙利算法](https://blog.codgician.pw/2018/03/09/the-hungarian-algorithm/)

另外，这道题有种贪心做法是错的，但是数据太水了……



# C: Cities

贪心。

既然要建连通图，那么显然每个点的点权至少要被算上一次（因为它们都一定需要与另一个点相连）。我们希望与每个点相连的点的权值都最小，那么我们只需要找出权值最小的点，并让剩下的点都与它相连就好了。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123C/greedy.cpp)



# D: Dance

DFS + 贪心。

要想让总魔法值最多，我们可以考虑尽量多选能带来最多魔法值的基础魔法来进行升级。我们可以首先对树 DFS 一次，算出并记录每种基础魔法（也就是叶子节点）升级到最高魔法所能带来的魔法值。接下来我们对它从大到小进行排序，依次模拟升级过程（又是一次 DFS）。

这个做法貌似不能称作正解，因为如果构造一棵 ” $\frac{N}{2}$ 个节点组成一条链，剩下 $\frac{N}{2}$ 个节点分别是前 $\frac{N}{2}$ 个节点的孩子“ 这样子的树，貌似可以把这个做法卡掉…… 还是出题人太良心了。🙈

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123D/dfs_greedy.cpp)



# E: Sequence

找规律。

要使删掉 $a_i$ 后总好数最多，也就是要让删掉 $a_i$ 对好数数量造成的影响最小。这里我们分两种情况讨论：

- 如果删掉的 $a_i$ 本身是好数，就意味着存在 $k < i, \ a_k < a_i$。换句话说，对于 $j > i$ 且 $a_j > a_i$ 的好数，就算把 $a_i$ 删掉了，依然有 $k < j, \ a_k < a_j$ 使得 $a_j$ 满足好数的定义。因此删掉 $a_i$ 只会让总好数数量减一；
- 如果删掉的 $a_i$ 不是好数，那么删掉 $a_i$ 会影响满足 $j > i, \ a_j > a_i$ 且在 $a_j$ 之前只有 $a_i < a_j$ 的数。删掉 $a_i$ 会让总好数数量减去满足那样条件的数的数量。

## 方法一

至此，一种显而易见的解决方式是使用树状数组。

我们可以开两个树状数组。第一个树状数组用于获取 $a_i$ 之前有多少个数小于 $a_i$，具体做法是读入 $a_i$ 后就将 $a_i$ 位置上的值改为 $1$，这样获取前 $a_i - 1$ 个位置的前缀和就是 $a_i$ 之前小于 $a_i$ 的数的个数了。我们记之为 $pre[i]$。

第二个树状数组用于获取 $a_i$ 后满足上面第二类情况里提到的满足特定条件的数有多少个。具体做法是倒着遍历一次序列，如果对于 $a_i$ 有 $pre[i] = 1$，就在树状数组中 $a_i$ 的位置上加 $1$。如果 $pre[i] = 0$，就意味着 $a_i$ 不是好数，我们获取第 $a_i + 1$ 个元素到第 $n$ 个元素的后缀和就是满足特定条件元素的个数了。我们记之为 $aff[i]$。 当然如果 $pre[i] = 0$ 的话就意味着 $a_i$ 是好数，那么 $aff[i] = 1$（删去它对总好数数量的影响为 $1$）。

然后遍历一遍序列，找出 $aff[i]$ 最小的 $a_i$ 即可。

复杂度：$\mathcal{O}(N\log{N})$

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123E/binary_indexed_tree.cpp)

## 方法二

如果进一步观察，我们可以意识到我们其实没有必要记录 $pre[i]$... 我们只需要记录 $a_i$ 之前的最小值和次小值，同时扫一遍序列就行了。

我们记 $\text{minVal}$ 为当前最小值，$\text{secMinVal}$ 为当前次小值。

- 如果 $a_i < \text{minVal}$，那么 $\text{minVal}$ 变成了 $a_i$，$\text{secMinVal}$ 变成了 $\text{minVal}$；
- 如果 $\text{minVal} \le a_i < \text{secMinVal}$，那么意味着对于 $a_i$ 在其之前且比 $a_i$ 小的数只有 $1$ 个。如果删去 $\text{minVal}$ 必然会影响 $a_i$，因此 $aff[\text{minVal}]++$。同时，又由于 $a_i$ 本身是个好数，因此删去 $a_i$ 也会少一个好数，$aff[a_i]++$；
- 如果 $a_i \ge \text{secMinVal}$，那么 $a_i$ 本身是个好数，删去它会少一个好数，所以 $aff[a_i]++$。

然后遍历一遍序列，找出 $aff[i]$ 最小的 $a_i$ 即可。

复杂度：$\mathcal{O}(N)$

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123E/observation.cpp)



# F: Four-tuples

容斥原理。

首先需要注意到的是，题目中事实上允许了 $x_1 = x_3$ 或是 $x_2 = x_4$。

我们首先需要求得下列 $12$ 种情况的方案数：

1. $x_1 \neq x_2 \neq x_3 \neq x_4$
2. $x_1 = x_2$
3. $x_2 = x_3$
4. $x_3 = x_4$
5. $x_4 = x_1$
6. $x_1 = x_2, \ x_3 = x_4$
7. $x_2 = x_3, \ x_4 = x_1$
8. $x_1 = x_2 = x_3$
9. $x_2 = x_3 = x_4$
10. $x_3 = x_4 = x_1$
11. $x_4 = x_1 = x_2$
12. $x_1 = x_2 = x_3 = x_4$

那么根据容斥原理，我们最终所要求的答案即：${1} - ({2} + {3} + {4}) + ({6} + {7}) + ({8} + {9} + {10} + {11}) - 3 \cdot {12}$。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123F/inclusion_exclusion_principle.cpp)



# G: Game

变形的限制物品个数的统计方案数的背包问题。

首先，我们知道 Nim 游戏先手必胜的局面为每堆石子数的异或和非 $0$。那么显然，后手必胜的局面就是每堆石子数的异或和为 $0$。那么本题实际上就是要求我们拿走不超过 $d$ 堆石子使得剩下每堆石子数的异或和为 $0$。

其次，我们需要知道异或运算的一个性质：$a \oplus a = 0$。另外， $a \oplus b \oplus a = b$。放在本题中，就意味着本题变为了：选出不超过 $d$ 堆石子，使得选出石子异或和与所有石子异或和相等。

这样一来，我们不妨把所有石子异或和 $\text{xorSum}$ 视作背包容量，每堆石子视作物品，并且选用物品个数不能超过 $d$。这就成了一个变形的背包问题，只不过总容量从加法和变成了异或和。

我们记录 $dp[i][j][k]$ 表示对于前 $i$ 件物品，从中选取 $j$ 件物品且已选取物品异或和为 $k$ 时的总方案数。不难写出如下状态转移方程：
$$
dp[i][j][k] = 
\begin{cases}
dp[i - 1][j][k] & j = 0 \\
dp[i - 1][j][k] + dp[i - 1][j - 1][k \oplus a_i] & j > 0 \\
\end{cases}
$$
至于初始化，我们初始化 $dp[0][0][0] = 1$。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123G/dp.cpp)



# H: Dominoes

BFS。

既然保证图中只有一个空格，那么相比考虑骨牌的移动我们不妨考虑这个空格的移动。这样子一来这道题就显得很简单了。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/Newcoder/123H/bfs.cpp)

