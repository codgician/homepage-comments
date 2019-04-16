---
uuid: 2e9f0b86-d85b-11e8-99eb-5f8a18b2e43c
title: 浅谈无旋转 Treap
date: 2018-07-28 15:15:01
updated: 2018-07-28 15:15:01
tags: 
  - Competitive Programming
  - Data Structure
  - Treap
category: Notes
---

# 简介

**Treap = Tree + Heap**。Treap 是一种**弱平衡**的二叉查找树。

我们知道相较普通的二叉查找树，平衡二叉树与之最显著的区别就是后者是 “平衡的”，能够有效防止前者退化成一条链的情况。平衡二叉树有很多种，比如 AVL、红黑树、SBT、Splay 等等， 而 Treap 是其中相对好写的一种。

顾名思义，**Treap 通过二叉堆的性质来维护二叉树的平衡**。一个二叉（大根）堆满足这样的性质：一个节点的两个儿子的值都小于节点本身。但是这样的规定看似与二叉搜索树的性质矛盾…… 解决这个问题的办法只有一个 —— 让每个节点有两个键值，其中一个满足二叉堆性质，另一个满足二叉搜索树性质。

因此，Treap 中的每个节点含有两个键值 $key$ 和 $value$，其中 $key$ 满足二叉堆的性质，而 $value$ 满足二叉搜索树的性质。$value$ 的值我们显然是无法随便改变的（因为它取决于用户输入），因此为了维护平衡性我们只能在 $key$ 上做文章。我们可以考虑给每个节点的 $key$ 都赋上随机值。由于主流的随机数生成算法（比如大素数取模）很难出现完全单调的序列，因此这样一来我们可以保证 Treap “基本平衡“。

你可能会发现 Treap 和 笛卡尔树 存在很多相似之处。的确，不过这两者最显著的区别是，Treap 的 $key$ 是随机值，而笛卡尔树的不是。

# 无旋转的 Treap

## 两个核心函数

一般的 Treap 是带有旋转操作的（正如大部分平衡树一样），而旋转操作的实现往往是我这种蒟蒻的噩梦。而无旋转的 Treap 却喜闻乐见地不通过旋转来维护其平衡性，所以相比普通的 Treap 要好写不少（也暴力不少）。下面我们首先来介绍无旋转 Treap 的两个核心操作：

- **merge (A, B)**：合并两棵子树（它们的根节点分别是 $A$ 和 $B$），复杂度 $\mathcal{O}(\log{n})$；
- **split (A, k)**：将以 $A$ 为根节点的树分裂为两棵子树：前者包含 $A$ 中的前 $k$ 小的节点（或者是小于 $k$ 的节点），后者包含剩余节点。复杂度 $\mathcal{O}(\log{n})$。

**注意：合并操作的前提是其中一棵 Treap 上的所有键值（即 $value$）小于另一棵 Treap 上的键值，否则只能启发式合并**！

有了这两个操作，我们就可以实现平衡树的大部分功能了。但在此之前，让我们先来看看如何实现这两个核心函数。

### 合并

我们假设现在维护的是满足**小根堆**性质的 Treap，记将被合并的两棵子树的根节点为 $A$ 和 $B$。

我们比较 $A$ 和 $B$ 的 $key$，谁的 $key$ 小谁就留在当前位置并成为另一者的祖先（小根堆性质）。我们假设 $A$ 的 $key$ 较小，那么 $A$ 的左子树显然不需要变动了（二叉搜索树性质），所以我们需要把 $A$ 的右子树和 $B$ 合并在一起并将得到的树作为 $A$ 的新右子树。合并完后还需要更新一下以 $A$ 为根子树的大小。

参考实现如下：

```cpp
int merge(int fst, int snd)
{
    if (!fst)
        return snd;
    if (!snd)
        return fst; // 如果其中一棵为空树，返回另一棵即可

    if (treap[fst].key < treap[snd].key)
    {
        treap[fst].rightSon = merge(treap[fst].rightSon, snd);
        update(fst); // 更新子树大小
        return fst;
    }
    else
    {
        treap[snd].leftSon = merge(fst, treap[snd].leftSon);
        update(snd);
        return snd;
    }
}
```

### 分裂

分裂其实有两种方式：

- 按排名分裂：前 $k$ 大的数构成一棵子树，剩余的构成另一棵；
- 按权值分裂：小于等于 $k$ 的数构成一棵子树，剩余的构成另一棵。

接下来我们主要演示按权值分裂（按排名分裂基本同理）。

我们考虑到当前点 $cntPt$，我们只需要考虑 $cntPt$ 是需要跟左子树拆成一堆还是跟右子树拆成一堆。如果我们发现当前点的权值小于等于 $k$，那么它和它的左子树都应该被并入小于等于 $k$ 的数构成的子树。接下来，我们还需要在它的右子树中把小于等于 $k$ 的部分拆出来，递归下去。

参考实现如下：

```cpp
void split(int cntPt, int val, int & fst, int & snd)
{
    if (!cntPt)
    {
        fst = 0;
        snd = 0;
        return;
    }

    if (treap[cntPt].value <= val)
    {
        fst = cntPt;
        split(treap[cntPt].rightSon, val, treap[cntPt].rightSon, snd);
    }
    else
    {
        snd = cntPt;
        split(treap[cntPt].leftSon, val, fst, treap[cntPt].leftSon);
    }
    update(cntPt);
}
```

## 运用核心函数

我们不妨先直接拉一道家喻户晓的模板题来举个例子：[普通平衡树 - 题目 - LibreOJ](https://loj.ac/problem/104)。

题目中要求我们实现 $6$ 类操作：

1. 插入数 $x$；
2. 删除数 $x$（若有多个相同的 $x$ 则只删除一个）；
3. 查询数 $x$ 的排名（若有多个相同的 $x$ 则输出最小排名）；
4. 查询排名为 $x$ 的数；
5. 求 $x$ 的前驱（即小于 $x$ 的数中的最大值）；
6. 求 $x$ 的后继（即大于 $x$ 的数中的最小值）。

下面我们就试着主要靠两个核心函数来解决上述 $6$ 个问题。

### 插入

 首先我们将带插入节点的 $key$ 赋成随机值，然后按 $x$ 将 Treap **split** 成小于等于 $x$ 的 $A$ 和大于 $x$ 的 $B$。我们不妨把待插入的 $x$ 看成一棵只含一个节点的 Treap，先将其与 $A$ **merge**，再得到的 Treap 与 $B$ **merge**，就完成插入了。

### 删除

与插入类似，首先我们按 $x$ 将 Treap **split** 成小于等于 $x$ 的 $A$ 和大于 $x$ 的 $B$，再将 $A$ **split** 成小于等于 $x - 1$ 的 $C$ 和大于 $x - 1$（即等于 $x$）的 $D$。接下来，我们将 $B$ 和 $C$ **merge** 在一起即可。

上述操作会把所有等于 $x$ 的数全部删掉。如果题目只要求删掉一个 $x$，可以考虑将 $D$ 的左子树和右子树合并在一起得到 $E$。这样一来就等于舍弃了 $D$ 的根节点，使得 $D$ 中等于 $x$ 的数少了一个。最后再把 $E$ 也 **merge** 进答案。

### 查询数的排名

实际上就是查询有多少个数比 $x$ 小，然后将结果加 $1$ 即是排名。

考虑现按 $x - 1$ 将 Treap **split** 成 $A$ 和 $B$，这样一来 $A$ 的大小加上 $1$ 就是我们需要的答案。查询完后再将 $A$ 和 $B$ **merge** 起来恢复原状即可。

### 查询排名对应的数

这个操作看起来不能仅凭两个基本操作完成了，我们需要引入一个新函数（返回的是下标）。具体实现也十分简单，就是在当前子树中查询第 $k$ 小值，看看代码就能理解了。参考代码中给出非递归版的实现：

```cpp
int findValByRank(int cntPt, int rnk)
{
    while (true)
    {
        if (rnk <= treap[treap[cntPt].leftSon].size)
            cntPt = treap[cntPt].leftSon; // 若当前子树大小 + 1大于 rnk，则在左子树中查第 rnk 大
        else if (rnk == treap[treap[cntPt].leftSon].size + 1)
            return cntPt; // 若当前子树大小 + 1 等于 rnk，那就是要找的啦
        else
        {
            rnk -= treap[treap[cntPt].leftSon].size + 1;
            cntPt = treap[cntPt].rightSon; // 若当前子树大小 + 1 小于 rnk，则在右子树中查第 rnk - (当前子树大小 + 1) 大
        }
    }
}
```

### 查询前驱或后继

这里我们以查询前驱为例（后继完全同理）：

我们考虑先将 Treap 按 $x - 1$ **split** 成 $A$ 和 $B$，那么 $A$ 树中最大的数（即第 $A.size$ 大）就是我们要查询的答案。借用上面的查询排名对应数的函数即可实现。

### 参考代码

[完整参考代码](https://github.com/codgician/Competitive-Programming/blob/master/LOJ/104/treap_without_rotations.cpp)

# 参考文献

- chen_tr - [偷懒专用平衡树——Treap](https://blog.csdn.net/chen_tr/article/details/50924073)
- LadyLex - [无旋treap：从好奇到入门](https://www.cnblogs.com/LadyLex/p/7182491.html)
- yyf0309 - [无旋转Treap简介](https://www.cnblogs.com/yyf0309/p/Unrotated_Treap.html)
