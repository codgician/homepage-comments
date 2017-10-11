---
title: 水题中的教训
date: 2017-10-08 15:53:10
tags: 
- ACM-ICPC
category: Notes

---

# 前言

在 HDU，想报名进 ACM 集训队水水都有条件，不在 HDUOJ 上水上几十道题连报名考试的机会都木有……于是为例尽快达到要求题量我就趁国庆的时间上 HDUOJ 专挑水题做，本以为专做水题 AC 率应该很养眼的，结果事实证名我自己什么水平心里果然一点 char(66) 数都没有。



# 犯得最多的蠢错



## 浮点数的精度丢失

比如 HDUOJ: [2001](http://acm.hdu.edu.cn/showproblem.php?pid=2001), [2023](http://acm.hdu.edu.cn/showproblem.php?pid=2023)

浮点的内部存储方式是科学记数法，具体存储时（以单精度为例）32位被分成了三个部分：符号位（1位），阶码（8位），尾码（23位）。其中符号位存储的是正负，阶码存储的是指数，尾码存储的是有效数字的小数部分。

当指数变大时连续两数间的间隙也会越来越大，导致精度丢失（即指数相同、有效数字相邻的两浮点数实际值之间的差距越来越大，当差距大到一定程度时就会导致明显的精度丢失）。而双精度由于尾码更长，所以在指数相同时精度丢失的情况比单精度少不少。

去学习了一圈后写了一篇笔记 https://codgician.github.io/2017/08/18/on-int-and-float。



## 运算过程中一不小心溢出

比如 HDUOJ: [1001](http://acm.hdu.edu.cn/showproblem.php?pid=1001)

在相乘的时候实际上就已经有可能超过 int32 上限了，而这道题数据精妙到即使使用 int64 也毫无卵用。所以就要先除后乘，不过除之前还一定要保证能够整除，否则……

 

## 担心溢出 先除后乘出现精度问题

比如 HDUOJ: [2032](http://acm.hdu.edu.cn/showproblem.php?pid=2032)

我可能有毒。这道题还是不优化了吧（逃



## 循环完后搞忘移动指针指向下一位

比如 HDUOJ: [2034](http://acm.hdu.edu.cn/showproblem.php?pid=2034)

你怎么不上天！



## 没有看清输入/输出格式

比如 HDUOJ: [1000](http://acm.hdu.edu.cn/showproblem.php?pid=1000), [1096](http://acm.hdu.edu.cn/showproblem.php?pid=1096), [2018](http://acm.hdu.edu.cn/showproblem.php?pid=2018), [2027](http://acm.hdu.edu.cn/showproblem.php?pid=2027) 

我都不好意思说我 A + B 问题都 TM 提交了 8 次才 A 掉， 原因就是带着 NOIP 的思维惯性做题而没有注意题目要求的输入格式。

在 HDUOJ 上输入格式大致有三种：

1. 循环输入有效数据直到 EOF 的；
2. 循环输入数据但遇到 0 或 -1 就停止的；
3. 先输入数据组个数再输入数据的。

输出格式也大致有三种:

1. 数据组和数据组间无空行的
2. 数据组和数据组间有空行且最后一组数据不能多输出空行的。
3. 每组数据后都要跟一个空行的。

orz 以后一定要看清楚啊……



## 微小的笔误

比如 HDUOJ: [2001](http://acm.hdu.edu.cn/showproblem.php?pid=2001)

笔误，把 x 误写作 ans。



## 找最值时忘记初始化已知最值

比如 HDUOJ: [2025](http://acm.hdu.edu.cn/showproblem.php?pid=2025)

捂脸。



## 蠢哭

比如 HDUOJ: [1106](http://acm.hdu.edu.cn/showproblem.php?pid=1106)

组合多位数时乘以 10 的幂次序搞反了，即最高位乘了 $10^0$，次高位乘了 $10^1$，以此类推…… 然后看了半天没看出来。



# 郑重声明

**以上错误再犯就穿女装一天**

不过好像我不告诉你我犯了错你也不知道啊（逃