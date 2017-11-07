---
title: NOIP 2005 - 过河
date: 2017-11-07 12:22:00
tags: 
- ACM-ICPC
- Dynamic Programming
category: Notes
---

# 题面

注：数据范围中的 109 实际上是 $10^9$。 :confounded:

[HDUOJ - 4842: 过河](http://acm.hdu.edu.cn/showproblem.php?pid=4842)



注：本人癖好特殊，为了使得代码可读性更高，本人写的代码中变量名都使用了驼峰法命名。

`bridgeLength`：独木桥的长度 L

`minHop`：蛤一次跳跃的最短距离 S

`maxHop`：蛤一次跳跃的最长距离 T

`stoneNum`：桥上石头个数 M



# 首先，这显然考动态规划

我们可以很容易写出状态转移方程：
$$
f(i) = min\{f(i), f(i - j) + isStone(i) \ | \  minHop \leq j \leq maxHop\}
$$
简单地写出伪代码：

```cpp
for (int i = 1; i <= bridgeLength; i++)
  for (int j = minHop; j <= maxHop && j <= i; j++)
    dp[i] = min(dp[i], dp[i - j] + isStone[i]);
```

注：其中 `isStone[i]` 代表第 $i$ 位置是否有石头，0 为没有，1 为有。



于是你瞬间兴奋了起来，大呼这道大水题不一次A掉就穿女装……

然后你默默地打开了淘宝……



# 特殊情况的优化

 我们注意到题目中给出的取值范围 `1 <= minHop <= maxHop <= 10`，也就是说，蛤的最短跳跃距离 `minHop` 是可以等于最长跳跃距离 `maxHop` 的……

在这种情况下，蛤是没有选择的，只有一种跳法，即每次都跳 `maxHop` 那么长，遇到了石头…… 也只能踩上去。这种情况还用什么动态规划呢……

```cpp
if (minHop == maxHop)
{
    int ans = 0;
    for (int i = 1; i <= stoneNum; i++)
        if (stone[i] % maxHop == 0)
            ans++;
    cout << ans << endl;
}
```



# 路径压缩

但特殊情况毕竟也是特殊情况，看看数据范围吧，要真开 $10^9$ 的数组不 MLE 也会 TLE。这个时候，我们就需要考虑路径压缩了。

再次观摩数据范围，我们发现，虽然桥可以长达 $10^9$，但石头最多却只有区区 $100$ 个。也就是说，在该桥上**石头是很稀疏的**，存在大量的空白区域，而这些空白区域则是浪费时间的元凶。

但是我们首先也要注意，对于刚刚讲到的 `minHop == maxHop` 的情况由于步长固定，是不可以压缩的。

---

我们不妨假设青蛙一次跳跃的距离为 $p$ 或者 $p + 1$（均为整数），在经历了 $x$ 次距离为 $p$ 的跳跃和 $y$ 次距离为 $p + 1$ 的跳跃后移动的总距离为 $Q$。那么有：
$$
px + (p + 1)y = Q
$$


我们知道，$p$ 和 $p + 1$ 的最大公约数 $gcd (p, p + 1) = 1$，所以由拓展欧几里得算法，很容易得知对于方程：
$$
px + (p + 1)y = gcd(p, p + 1) = 1
$$
一定存在整数解。

那么显然，对于方程：
$$
px + (p + 1)y = Q
$$
也就一定存在整数解咯。



我们不妨假设我们得到了一组整数解 $x_0$ 和 $y_0$。并且，我们假设 $ 0 \leq x_0 \leq p$。

当 $Q \geq p(p + 1)$ 时，将这组解带入原方程并变换，易得：
$$
\begin{aligned}
& y_0 = \frac{Q - px_0}{p + 1} \\
& \geq \frac{Q - p^2}{p + 1} \\
& \geq \frac{p(p + 1) - p^2}{p + 1} \\
& = \frac{p}{p + 1} \geq 0
\end{aligned}
$$

也就是说,，当 $Q \geq p(p + 1)$ 时，二元一次方程 $px + (p + 1)y = Q$ **一定存在正整数解**。

---

回到题目中来，上述的结论告诉我们，如果蛤每次走 `maxHop -1 ` 或者 `maxHop` 步的话，一定可以走到 `(maxHop - 1) * maxHop` 以及之后的所有点。那么，到达这之后的所有点（下一个石头之前）所踩石头的最少数目都是一样的。我们自然也可以不用计算它们了。

那么我们就可以得到路径压缩的办法：如果两石头间的距离大于`(maxHop - 1) * maxHop`，那么我们只需要把它们间的距离改成 `(maxHop - 1) * maxHop` 就可以了。

```cpp
sort(stone, stone + stoneNum + 1);

int k = maxHop * (maxHop - 1);
for (int i = 1, j = 0; i <= stoneNum; i++)
{
    stone[i] -= j;
    int delta = stone[i] - stone[i - 1];
    if (delta > k)
    {
        j += delta - k;
        stone[i] = stone[i - 1] + k;
    }
}
```



[完整参考代码](https://github.com/codgician/ACM/blob/master/HDUOJ/4842/dp.cpp)



# 参考文献

yuyanggo - [noip2005 过河 （数论+动态规划）](http://blog.csdn.net/yuyanggo/article/details/48341259)

JmingS - [hdu 4842（NOIP 2005 过河）之 动态规划（距离压缩）](http://www.cnblogs.com/shijianming/p/5149559.html)