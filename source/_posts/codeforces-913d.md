---
title: Codeforces 913D - Too Easy Problems
date: 2018-01-09 20:54:00
tags: 
- ACM-ICPC
- Greedy
- Binary Search
- Codeforces
category: Solutions
mathjax: true
---

# 题面

什么也别说先让我哭一会儿，嗷呜呜呜呜呜呜~ :sob:

[题目链接](http://codeforces.com/contest/913/problem/D)

大致题意：

某场考试时长为 $T \ (1 \le T \le 10^9)$ 毫秒，并且由 $n \ (1 \le n \le 2 \cdot 10^5)$ 道题目组成（每题 $1$ 分）。对于第 $i$ 道题目，你可以选择花费 $t_i \ (1 \le t_i \le 10^4)$ 毫秒来解出它或是直接跳过它。

老师见你很皮，于是认为部分题目对你而言太简单了。所以，对于每一道题目 $i$ 老师都规定了一个指数 $a_i \ (1 \le a_i \le n)$，并表示只有当你解出的总题数不超过 $a_i$ 时第 $i$ 题才能为你带来得分，否则就算你做出了第 $i$ 题该题分数也不会计入总分。

你显然希望获得最大分数，那就看你选择做哪几道题咯（如果有多种答案随便输出一种就好）~



# 贪心！

首先，我们假设我们能获得的最大分数为 $k$。不难发现，花费时间去做那些并不能给自己带来分数的题目是毫无意义的。因此，一定存在做出题目数量恰好为 $k$ 并且得分为 $k$ 的方案。

那么我们就找这种方案就好咯~

首先，我们需要把 $k$ 的大小确定下来。

对于任意一个总分 $m$，只要我们能找到 $m$ 道题使得它们所耗时间之和小于等于考试总长并且每道题的 $a_i$ 值都大于等于 $m$，那么这个 $m$ 就是可行的。在具体实现过程中仅需要把题目都按花费时间从小到大的顺序排个序，然后依次遍历，只要满足 $a_i \ge m$ 就做它，如此一直到时间耗尽，最后比对一下所做的题数是否大于等于 $m$ 即可。而 $k$ 就是最大的可行的 $m$。

在寻找到 $k$ 后输出方案就很容易咯（在上述验证 $k$ 可行性的过程中实际上都已经把方案找出来了... 唯一要注意的是输出原始题号而非排序后的题号，所以结构体中要存储原始题号）~



# 具体实现

## 朴素

把 $k$ 从 $0 \sim n$ 遍历一遍以确定 $k$ 的大小。

复杂度 $\mathcal{O}(n^2)$ ，看看数据范围就知道多半要超时，我就没写了（逃

## 二分搜索

[Submission #34048622](http://codeforces.com/contest/913/submission/34048622)

[GitHub - Backup Link](https://github.com/codgician/ACM-ICPC/blob/master/Codeforces/913D/greedy%2Bbinary_search.cpp)

由朴素很容易就能想到二分搜索。我们只需要对 $k$ 在 $0 \sim n$ 间进行二分搜索就好了，这样的复杂度是 $\mathcal{O}(nlogn)$。

## 其它

挖坑待填……



# %%%

-   tourist - [Hello 2018 -- Tutorial](http://codeforces.com/blog/entry/56992)

