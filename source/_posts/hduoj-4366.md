---
title: HDUOJ 4366 - Successor
date: 2018-04-10 12:16:01
tags: 
- ACM-ICPC
- Data Structure
- Segment Tree
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

Sean 是一位霸道总裁，在他的公司里有 $n$ 个员工（含他自己），里每一位员工都有一位上级。与此同时，每一位员工都有一个忠诚值 $\text{loyalty}$ 与能力值 $\text{ablilty}$。**每个员工的忠诚值 $\text{loyalty}$ 均不同**。Sean 超喜欢炒鱿鱼，如果一位员工被炒了，那么他将会被他下属中能力值高于他并且忠诚度最高的下属代替。Sean 想进行 $m$ 次询问：如果他炒了某个员工，谁会来接替他。

**数据范围**：

$2 \le n \le 50000$

$2 \le m \le 50000$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4366)

# 分析

我们注意到，在炒掉员工 $i$ 时我们需要查询其下属中 $\text{loyalty}$ 最大的员工。我们很自然地想到用线段树来维护区间内最大的忠诚值。

但是…… 呈现在我们眼前的貌似是一棵树…… 那也没有关系，我们只需要用 DFS 计算其 DFS 序从而将其“线性化”。根据 DFS 序的性质我们知道，对于节点 $i$，其子树含有的节点就是节点 $i$ 在 DFS 序中第一次出现位置和第二次出现位置之间出现的节点。在具体实现上，我们可以记录每个节点第一次出现的时间戳 $\text{inTime}[i]$ 和 第二次出现的时间戳 $\text{outTime}[i]$。那么所有满足 $\text{inTime}[i] \le \text{inTime}[j] \le \text{outTime}[i]$ 的节点 $j$ 就是 $i$ 的下属了。为了处理方便，本题中我们使 $\text{inTime}$ 连续，换句话说，就是在计算 $\text{outTime}$ 时不增加时间。

于是，我们用线段树维护的区间下标实际上是时间戳，而维护的值就是在两个时间戳之间的最大 $\text{loyalty}$ 值。显然，每一个 $\text{inTime}$ 都对应一个员工，而区间 $[\text{inTime[i]} + 1, \text{outTime}[i] - 1]$ 中的最大忠诚度也就是员工 $i$ 下属中的最大 $\text{loyalty}$。然后，根据每个员工的 $\text{loyalty}$ 不同，我们可以用一个 $\text{map}$ 来建立 $\text{loyalty}$ 和 $\text{id}$ 间的对应关系以快速查询 $\text{id}$。

最后的一个问题，就是题目对 $\text{ability}$ 也有限制：接替被炒员工的下属必须 $\text{ability}$ 要高于被炒员工。其实这也很好解决，我们可以开始时将线段树初始化成一个空树，然后将所有员工按 $\text{ability}$ 排序，然后按照 $\text{ability}$ 从大到小顺序插入线段树。这样就可以保证在查询员工 $i$ 时，线段树中不存在 $\text{ability}$ 比当前员工小的员工。由于不同的查询之间是不会相互影响的，这也要求我们提前预处理所有答案，然后以 $\mathcal{O}(1)$ 的时间复杂度回答每一次询问。

# 实现

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/4366/segment_tree.cpp)

