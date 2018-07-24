---
title: HDUOJ 6304 - Chiaki Sequence Revisited
date: 2018-07-24 14:47:23
tags: 
- ACM-ICPC
- Observation
- Mathematics
- Binary Search
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

定义一个序列 $a$：
$$
a_n = 
\begin{cases}
1 & n = 1, 2 \\
a_{n - a_{n - 1}} + a_{n - 1 - a_{n - 2}} & n \ge 3
\end{cases}
$$
记序列 $a$ 的前缀和 $s_i = \sum\limits_{k = 1}^{i} a_k$。现共有 $T$ 组数据，每组数据给定 $k$，试求 $s_k$ 对 $10^9 + 7$ 取模后的值。

**数据范围**：

$1 \le T \le 10^5$

$1 \le n \le 10^{18}$

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6304)

# 找规律...

我们先对 $a_i$ 打个表吧（逃

[前 10000 个值](https://pastebin.com/LDBBb26K)

我们观察这些值，不难发现从第 $2$ 个值开始，$1$ 出现了 $1$ 次，$2$ 出现了 $2$ 次，$3$ 出现了 $1$ 次，$4$ 出现了 $3$ 次…… 好像所有奇数值只会出现 $1$ 次，而能被 $2$ 整除但不能被 $4$ 整除的都出现 $2$ 次，能被 $4$ 整除但不能被 $8$ 整除的都出现 $3$ 次…… 如果我们结合每个数的二进制再观察一番，就会发现，除 $1$ 以外，值 $i$ 会连续出现 $f(i) = \log_{2}{\text{lowbit}(i)} + 1$ 次。 

如何理解 $f(i)$ 这个式子呢？$\text{lowbit}(i)$ 就是 $i$ 的二进制表示中最低位 $1$ 即它后面的 $0$ 组成的数。对这个数取对数，自然就是最低位 $1$ 的位数了。举个例子，对于数 $10$，其二进制为 $(1010)_2$，则 $\log_2{\text{lowbit}(10)} + 1 = 2$ 。观察到最低位 $1$ 也出现在第 $2$ 位（从右往左）。

由此一来，我们首先需要知道对于 $s_k$，其中完整出现了 $f(i)$ 次的最大 $i$ 是多少。如果我们记之为 $t$，那么 $s_k$ 实际上可分为前 $t$ 完整出现的值的和，外加上 $t + 1$ 这一未完整出现值的和（如果有的话），即：  
$$
s_k = \sum\limits_{i = 1}^{t} (i \cdot f(i)) + (k - \sum_{i = 1}^{t} f(i)) \cdot (t + 1)
$$

至于 $t$ 的确定，我们可以考虑二分答案，借助：

$$
\sum\limits_{i = 1}^{t} f(i) = \lfloor \frac{t}{2^0} \rfloor + \lfloor \frac{t}{2^1} \rfloor + \lfloor \frac{t}{2^2} \rfloor + \dots
$$

为什么？我们前面发现的规律就是：对于能被 $2^i​$ 整除但不能被 $2^{i + 1}​$ 整除的数 $i​$，有 $f(i) = i + 1​$。我们现在不妨把 $f​$ 想象成一堆桶。上面的式子中，$\lfloor \frac{t}{2^0} \rfloor​$ 相当于给所有的数的 $f​$ 都加了 $1​$，$\lfloor \frac{t}{2^1} \rfloor​$ 相当于给所有能被 $2^1​$ 整除的数的 $f​$ 都加了 $1​$，…… 这样加完后，就满足规律所言了。

另外二分时要注意两点：

- 不可以直接用等比数列求和公式，因为每一项都向下取整了；
- 关于答案范围：通过前面的表可以观察到 $t$ 的值一般在 $\frac{k}{2}$ 左右，且很多时候略大于 $\frac{k}{2}$，故~~为了卡进时限~~可以将二分范围确立为 $[\frac{k}{2} - 1, \frac{k}{2} + 50]$。

至于完整部分的求和，我们不难发现如下规律：

- 出现次数为 $1$ 的数：$1, \ 3, \ 5, \ 7, \ 9, \ \dots$ 是个首项为 $2^0$，公差为 $2^1$ 的等差数列。
- 出现次数为 $2$ 的数：$2, \ 6, \ 10, \ 14, \ 18, \ \dots$ 进一步，可化为：$2^1 \times (1, \ 3, \ 5, \ 7, \ 9, \ \dots)$
- 出现次数为 $3$ 的数：$4, \ 12, \ 20, \ 28, \ 36, \ \dots$ 进一步，可化为：$2^2 \times (1, \ 3, \ 5, \ 7, \ 9, \ \dots)$
- $\dots$
- 出现次数为 $i$ 的数：$2^{i - 1} \times (1, \ 3, \ 5, \ 7, \ 9, \ \dots)$

所以我们用等差数列求和公式即可得解。

至于不完整部分的求和，我们都知道剩下的数全都是 $t + 1$ 并且个数只有 $k - \sum_{i = 1}^{t} f(i)$ 个，所以值就是 $ (k - \sum\limits_{i = 1}^{t} f(i)) \cdot (t + 1)$。

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/6304/observation_math.cpp)



# %%%

- wyj_alone_smile - [HDU 6304（找规律）](https://blog.csdn.net/wyj_alone_smile/article/details/81177111)
- purple_bro - [HDU - 6304 Chiaki Sequence Revisited](https://blog.csdn.net/purple_bro/article/details/81177315)