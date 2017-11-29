---
title: 浅谈动态规划
date: 2017-10-29 20:00:00
tags: 
- Computational Thinking
- ACM-ICPC
- Dynamic Programming
category: Notes
---

# 前言

总结动态规划…… 是个艰巨的任务。我希望能在两周之内完成。

在前一篇文章 [浅谈递推](https://codgician.github.io/2017/10/28/recursion-algorithm/) 我们简单谈了谈递推问题。在谈论今天的话题之前，我们不妨先回忆一下递推。

递推中最重要的思想即**将大问题转化为较小的子问题，并将子问题的答案计算出后存储起来以推出下一个问题（避免重复计算）。**但事实上，递推只是解决问题的手段罢了，而其思想，事实上就是动态规划。



# 动态规划

我们先看看 Wikipedia 上对于 Dynamic Programming 的定义吧：

> **Dynamic programming** (also known as **dynamic optimization**) is a method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions.

动态规划就是通过**拆分问题**，定义状态和状态之间的关系，使得问题能通过递推的手段去解决。所以，动态规划的核心事实上在于**状态的定义**和**状态转移方程式的定义**。

那么哪类问题可以用动态规划解答呢？

1. 具有**最优子结构**，也就是说每个阶段的最优状态一定可以由之前的某个阶段的某个或某些状态得到。换句话说，问题的最优解所包含的每一个子问题的解都是最优的。
2. 具有**无后效性**，也就是说如果当前若干个状态已经得到，那么此后的演变就只跟这若干个状态有关系，而不管它们是如何得来的。




# 二维动态规划



# 区间动态规划




# 参考文献

[HDUOJ](http://acm.hdu.edu.cn)

刘春英老师 - [ACM程序设计：动态规划](http://acm.hdu.edu.cn/forum/read.php?tid=3133)

王勐 - [什么是动态规划？动态规划的意义是什么？](https://www.zhihu.com/question/23995189/answer/35429905)

徐凯强 Andy - [什么是动态规划？动态规划的意义是什么？](https://www.zhihu.com/question/23995189/answer/35324479)

常敲代码手不抖 - [教你彻底学会动态规划——入门篇](http://blog.csdn.net/baidu_28312631/article/details/47418773) 