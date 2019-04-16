---
uuid: 8a93c77c-d853-11e8-a68d-cb37d861e4a7
title: HDUOJ 6240 - Server
date: 2018-07-22 20:01:23
updated: 2018-07-22 20:01:23
tags: 
  - Competitive Programming
  - Mathematics
  - Fractional Programming
  - Binary Search
  - HDUOJ
category: Solutions
---

# 题面

现有 $n$ 台服务器，第 $i$ 台服务器可在第 $S_i$ 天至第 $T_i$ 天使用，同时需要花费 $A_i$ 元。另外，第 $i$ 台服务器有一优惠指数 $B_i$，如果记选用的所有方案组成的集合为 $S$，那么最后需支付的总费用即 $\frac{\sum_{i \in S} A_i}{\sum_{i \in S} B_i}$ 元。现我们一共需要租赁 $t$ 天，要保证每一天都至少有一台可使用的服务器，试问最少花费。

**数据范围**：

$1 \le N, t \le 100000$

$1 \le S_i \le T_i \le t$

$1 \le A_i, B_i \le 1000$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6240)

# 分析

看看总费用的计算式就知道这是一道考察 0-1 分数规划 的题目…… 唯一不同之处是附加了一个条件：选用的服务器必须完全覆盖总时间区间 $[1, t]$。由于我只会二分答案来解决 0-1 分数规划问题，所以这里就只提供这种解法的题解（逃...

至于裸的 0-1 分数规划问题如何用二分答案的方法求解本文不再详讲。既然要在此基础之上满足覆盖总时间区间，无非就是在对答案进行判定时（记当前待判定的答案是 $mid$），光贪心取完所有满足 $\frac{A_i}{B_i} \le mid$ 的服务器是不够的，我们还需要在此基础上再取一些服务器使得整个时间区间都被覆盖。为了做到这一点，我们可以首先将服务器按照所覆盖时间段的左端点从小到大排序，然后引入一种能够维护前缀最小值的数据结构（树状数组或线段树皆可），维护可完整覆盖时间区间 $[1, k]$ 内需额外选择**不**满足 $\frac{A_i}{B_i} \le mid$ 的服务器的最小总花费。最后查询 $[1, t]$ 就是为了确保完整覆盖时间区间还需额外租赁的服务器的最小总花费。

# 实现

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/HDUOJ/6240/binary_search_fp_binary_indexed_tree.cpp)

# %%%

- zstu_zy - [hdu 6240](https://blog.csdn.net/zstu_zy/article/details/78661382)