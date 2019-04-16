---
uuid: c9cf9154-d84f-11e8-a65e-fb4ca51a622c
title: "Codeforces 1000F - One Occurrence"
date: 2018-07-11 20:05:00
updated: 2018-07-11 20:05:00
tags: 
  - Competitive Programming
  - Data Structure
  - Segment Tree
  - Persistent Segment Tree
  - Codeforces
category: Solutions
---

# 题面

现有一个长度为 $n$ 的数组 $a$ 和对其进行的 $q$ 次查询。第 $i$ 次查询给定一个区间 $[l_i, r_i]$，如果这个区间内存在只出现一次的数，输出任意一个满足这一条件的数；如果没有，则输出 $0$。

**数据范围**：

$1 \le n, a_i, q \le 5 \cdot 10^5$

[题目链接](http://codeforces.com/problemset/problem/1000/F)

# 离线做法

首先，我们来思考一下对于在区间 $[l, r]$ 内只出现一次的值应当满足什么性质。

我们记 $\text{last}[i]$ 代表 $a[i]$ 上一次出现的位置（如果不存在，我们记 $\text{last}[i] = 0$）。如果 $a[i] \ (l \le i \le r)$ 仅在区间 $[l, r]$内出现一次，那么一定满足：

- 在区间 $(i, r]$ 内没有 $a[i]$ 出现；
- $\text{last}[i] < l$ 或者 $\text{last}[i] = 0$。

通过上述性质的第二点，我们不难考虑到：**可以用线段树来维护区间内 $\text{last}[i]$ 的最小值**。因为如果区间内最小的 $\text{last}[i]$ 满足上述性质的话，我们就可以把 $a[i]$ 作为答案；而如果区间内最小的 $\text{last}[i]$ 都不满足上述性质的话，那么区间内也就没有满足上述性质的 $\text{last}[i]$ 了，自然也就没有只出现一次的数了。

但是，光维护最小值是不够的。我们仍然需要保证区间内最小的 $\text{last}[i]$ 满足刚才提到的第一点性质，即在区间 $(i, r]$ 内没有 $a[i]$ 出现。

我们不妨先来考虑一个简单一点的情形：假设数组的长度为 $n$，同时所有的查询区间都是 $[l, n]$。在这种情形下，我们只需要记不满足条件一的 $\text{last}[i] = +\infty$ 即可。换句话说，当且仅当 $(i, n]$ 中没有 $a[i]$ 出现时，$\text{last}[i]$ 才代表 $a[i]$ 上一次出现的位置。这样子，对于所有的区间 $[l, n]$，区间最小的 $\text{last}[i]$ 必然满足上述两点性质。

接下来我们再回到这个复杂的情形。如果我们事先对所有查询区间 $[l, r]$ 按照右端点 $r$ 从小到大排序，接下来随着 $r$ 的增长维护线段树就可以把复杂情形转移为上面说的简单情形了。详细地说，在查询区间 $[l_i, r_i]$ 时，线段树的规模只有 $r_i$，而下一步查询区间 $[l _{i + 1}, r_{i + 1}]$ 时，我们先在线段树中加入 $(r_i, r_{i + 1}]$ 间的信息使得线段树规模扩大到 $r_{i + 1}$ 后再进行查询。这样就可以保证，每次查询 $[l, r]$ 的时候，当前数组的长度实际上只有 $r$，所以就可以用上一段提到的方法解决问题：在第 $i$ 位加入新值后（这意味着 $a[i]$ 上一次出现的位置 $j$ 不再满足性质一），如果 $\text{last}[i] \neq 0$ 则令 $\text{last}[\text{last}[i]] = +\infty$。

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/Codeforces/1000F/segment_tree.cpp)

# 在线做法

思路与离线版类似，但是实现时使用可持久化线段树就可以在线了。

简而言之，查询区间 $[l, r]$ 时就等效于查询第 $r$ 个历史版本，而第 $i$ 个历史版本就代表原数组的 $[1, i]$ 部分对应的线段树。

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/Codeforces/1000F/persistent_segment_tree.cpp)

# %%%

- BledDest - [Educational Codeforces Round 46 Tutorial](http://codeforces.com/blog/entry/60288)
