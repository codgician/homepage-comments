---
title: HDUOJ 4089 - Activation
date: 2018-02-17 16:50:00
tags: 
- ACM-ICPC
- Dynamic Programming
- Probability
- HDUOJ
category: Solutions
#mathjax: true
---

# 题面

有 $N$ 个玩家要激活某种游戏，然而游戏的激活服务器对于这 $N$ 个玩家的激活请求只能一个一个处理（初始时主角 Tomato 排在第 $M$）。

对于队列中的第一个人，激活时可能遇到一下四种情况：

- 激活失败：队列不变，服务器将再次尝试激活当前玩家（概率 $P_\text{actFail}$）；
- 连接失败：当前玩家被扔至队尾（概率 $P_\text{connLost}$）；
- 激活成功：当前玩家离开队列（概率 $P_\text{actSuccess}$）；
- 服务中断：服务器炸了，无符继续处理任何请求（概率 $P_\text{down}$）。

求服务器服务中断并且此时 Tomato 排名 $\leq K$ 的概率（后文中记该事件为事件 $A$）。

[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=4089)


# 概率DP...

我们用 $dp[i][j]$ 来表示队列长度为 $i$ 且 Tomato 排在第 $j$ 时事件 $A$ 发生的概率。那么很显然，我们所要求的就是 $dp[N][M]$。

如何进行状态转移？我们不难推出：
$$
dp[i][j] = 
\begin{cases}
P_\text{actFail}dp[i][1] + P_\text{connLost}dp[i][i] + P_\text{down} & j =1 \\
P_\text{actFail}dp[i][j] + P_\text{connLost}dp[i][j - 1] + P_\text{actSuccess}dp[i - 1][j - 1] + P_\text{down} & 2 \le j \le k \\
P_\text{actFail}dp[i][j] + P_\text{connLost}dp[i][j - 1] + P_\text{actSuccess}dp[i - 1][j - 1] & j > k
\end{cases}
$$


接下来我们对其进行化简：
$$
dp[i][j] = 
\begin{cases}
\frac{P_\text{connLost}dp[i][i] + P_\text{down}}{1 - P_\text{actFail}} & j =1 \\
\frac{P_\text{connLost}dp[i][j - 1] + P_\text{actSuccess}dp[i - 1][j - 1] + P_\text{down}}{1 - P_\text{actFail}} & 2 \le j \le k \\
\frac{P_\text{connLost}dp[i][j - 1] + P_\text{actSuccess}dp[i - 1][j - 1]}{1 - P_\text{actFail}} & j > k
\end{cases}

$$


为了方便，我们不妨记：
$$
\begin{aligned}
P_\text{connLost}' & = \frac{P_\text{connLost}}{1 - P_\text{actFail}} \\
P_\text{actSuccess}' & = \frac{P_\text{actSuccess}}{1 - P_\text{actFail}} \\
P_\text{down}' & = \frac{P_\text{down}}{1 - P_\text{actFail}}
\end{aligned}
$$


因此，转移方程被进一步化简为：
$$
dp[i][j] = 
\begin{cases}
P_\text{connLost}'dp[i][i] + P_\text{down}' & j =1 \\
P_\text{connLost}'dp[i][j - 1] + P_\text{actSuccess}'dp[i - 1][j - 1] + P_\text{down}' & 2 \le j \le k \\
P_\text{connLost}'dp[i][j - 1] + P_\text{actSuccess}'dp[i - 1][j - 1] & j > k
\end{cases}

$$


好的，现在我们已经解决了转移方程的问题，接下来要考虑的是该怎么写代码咯。

观察转移方程，不难发现如果我们从 $i = 1 \rightarrow N$ 进行递推的话，当我们在计算 $dp[i]$ 时 $dp[i - 1]$ 事实上全是常量。因此，方程中除了含 $P_\text{connLost}'$ 的一项，剩余项均可看作常数项。

我们不妨将第 $j$ 个式子的常数项记作 $c[j]$，因此在计算 $dp[i]$ 时相当于求解 $i$ 元一次方程组：
$$
\begin{cases}
dp[i][1] & = P_\text{connLost}'dp[i][i] + c[1] \\
dp[i][2] & = P_\text{connLost}'dp[i][1] + c[2] \\
... \\
dp[i][i] & = P_\text{connLost}'dp[i][i - 1] + c[i] \\
\end{cases}
$$
$i$ 元一次方程组小学生都会解……
$$
\begin{aligned}
dp[i][i] & = P_\text{connLost}'dp[i][i - 1] + c[i] \\
& = P_\text{connLost}'(P_\text{connLost}'dp[i][i - 2] + c[i - 1]) + c[i] \\
& = \ ... \\
& = {P_\text{connLost}'}^{i - 1}dp[i][1] + {P_\text{connLost}'}^{i - 2}c[2] + ... + P_\text{connLost}c[i - 1] + c[i] \\
& = {P_\text{connLost}'}^{i}dp[i][i] + {P_\text{connLost}'}^{i - 1}c[1] + ... + P_\text{connLost}c[i - 1] + c[i]
\end{aligned}
$$

化简，得：
$$
dp[i][i] = \frac{\sum\limits_{j = 1}^{i}({P_\text{connLost}'}^{i - j} \cdot c[j])}{1 - {P_\text{connLost}'}^{i}}
$$


然后剩下的变量只需要递推求解一下就行咯。

接下来我们剩下的唯一问题就是 $c[j]$ 具体是什么了。我们也不难看出：
$$
c[j] = 
\begin{cases}
P_\text{down}' & j = 1 \\
P_\text{actSuccess}'dp[i - 1][j - 1] + P_\text{down}' & 2 \le j \le k \\
P_\text{actSuccess}'dp[i - 1][j - 1] & j > k
\end{cases}

$$


最后要注意的是，如果 $P_\text{down} = 0$，那概率肯定是 $0$ 啦，还算个毛线~

有了上述分析，我们就可以很容易地用递推写出代码了。



# 参考代码

[完整参考代码](https://github.com/codgician/ACM-ICPC/blob/master/HDUOJ/4089/dp.cpp)

P.S. 像我这种智商欠费的🐷在写概率 DP 时还是不要为了省一点空间作死下标从 $0$ 开始了…… 😭



# %%%

- morgan_xww - [Hdu 4089 Activation (概率dp) - 2011 ACM-ICPC Beijing Regional Contest Problem I](http://blog.csdn.net/morgan_xww/article/details/6920236)
- 将狼踩尽 - [HDU 4089 Activation（概率DP）](http://www.cnblogs.com/jianglangcaijin/archive/2013/05/04/3060411.html)
