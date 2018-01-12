---
title: 浅谈POSIX系统的权限管理设计
date: 2017-09-04 23:00:10
tags: 
- Codgic
- Unix
category: Notes
thumbnail: /2017/09/04/posix-permission-system/cover.jpg
mathjax: false
---

# 前言

经t123yh蛊惑决定在Codgic vnext中模仿一下POSIX的权限设计，结果自己想当然地写出来了一大坨翔 :/。于是还是赶紧提高一下自己的知识水平吧。



# 可行性？

题库，也就是存储题目的数据库。若把题目当作文件，那么不就是个文件系统了嘛（虽然细节上还是有很多不同）…… 

每一个题目也有对应的三种权限：**读、写、提交**。每种比赛也有三种对应权限：**读、写、参与**。这难道不就像是 POSIX 中的**读、写、执行**吗？

那么就完全可以效仿 POSIX ，用三位八进制（自动忽略 *SUID* 、*SGID* 和 *SBIT*）来存储权限，再另外存储所有者(owner)以及所有组(group)来实现权限管理。什么？你说有的 *nix 系统里面还有 ACL？那真是非常抱歉，我懒。（逃



# 概览

下面几节都是对参考文献赤裸裸的抄袭和翻译（我才不会告诉你还有删减，光速逃

在 POSIX 中，每一个文件都有一个所有者（owner）和一个所有组（group）。每一个尝试对文件进行操作的用户都会被分到一下三类中：

- `owner` （该用户即为所有者）
- `group`（该用户不是所有者，但隶属于所有组）
- `other`（其它人）

对于上述三类用户，每一类用户都有以下三种权限可以获得：

- `read`（读）
- `write`（写）
- `execute`（执行）

这样一来，你便可以设置 3 * 3 = 9 个权限，并且它们可以任意组合（虽然有的组合看起来很*狗）。

对于文件，访问数据都需要 *read* 权限，修改数据都需要 *write* 权限，执行都需要 *execute* 权限。

在 *nix 系统中，目录也是一种文件，故其采用的权限系统自然跟常规文件是一样的。**值得注意的是，赋给目录的权限并不会被目录中的文件自动继承。**

对于目录来说，列出目录中的文件列表需要对该目录有 *read* 权限（而并不是对目录中的文件有 *read* 权限）。在目录中添加、删除或重命名文件则需要对该目录有 *write* 权限（同样也并不是对目录中的文件有 *write* 权限 [对，你没有看错~]）。*execute* 权限显然不能直接应用到目录（毕竟它们又不是程序），显然这个权限另有用途。

*execute* 权限就被决定你是否能够 `cd` 进这个目录（或者说，决定你能否将该目录作为你的工作目录）。

另外，访问该目录内文件的 "inode" 信息（也就是文件的 metadata？）也需要 *execute* 权限。在搜索一个目录的过程中也需要读取该目录内文件的 "inode" 信息，所以人们通常也将对于目录的 *execute* 权限称作 *search* 权限。

> 插播广告：[理解inode - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2011/12/inode.html)。（广告位不招租）

另外：

> 你可以这样理解对于目录的 *read* 和 *execute* 权限：
>
> 目录实际上就是一个存储了目录内 每一个文件的名称 和 "inode" 信息 的文件。你需要 *read* 权限来访问每一个文件的名称。如果你已经知道每一个文件的名称，还需要 *execute* (aka *search*) 权限来访问每一个文件的 "inode" 信息。

*search* 权限实际上在很多常见的情况下都会用到。比如，命令 `cat /home/user/foo` 首先显然需要对 `foo` 文件的的 *read* 权限。但是除非你有对目录 `/` 、`/home` 和 `/home/user` 的 *search* 权限，`cat` 将无法找到文件 `foo` 的 "inode" 信息，自然也就没法读取它！你需要有对于所有父目录的 *search* 权限才能访问该目录或文件的 "inode" 信息，并且若你无法读取到该文件的 "inode" 信息也将无法读取该文件。

事实上很多其它命令也需要访问文件的 "inode" 信息才能工作。比如前文提到了我们需要目录的 *write* 权限才能在目录中添加、删除、重命名文件。但是这些操作依然需要修改（或至少访问）受波及文件的 "inode" 信息，故 *search* 权限也是必需的。

除去上述标准权限，其实还有额外的三种属性可以被设置在任何文件上（通常这些也被视作权限）：

- The set user ID (SUID)
- The set group ID (SGID)
- The text (aka sticky) attributes

最后，某些系统种还存在非标准的属性和附加的权限（比如 *Access Control List (ACL)*）。并不是所有 *nix 系统上都有这些玩意儿。



# 细节

每一个文件或目录都包含 12 个可被设置的权限比特 (*permisson bit, aka mode bit*)，意味着共有 $2 ^ {12} = 4096$ 种可能。这些比特要么是开（1）要么是关（0），并且均可以被独立设置。

所有的权限和属性信息都被放在文件的 *inode* 里面。只有文件的 `owner` 才可以修改（或者 root 用户）。注意，owner 并不需要额外设置权限来修改文件的权限。另外，在大部分 Unix 或 Linux 操作系统上只有超级用户 root 才能修改一个文件的所有者。

Unix 对继承权限的想法并不感冒。所以，与其它系统不同的是（说你呢，Windows），仅对目录拥有 *read* 权限并不会给你偷窥目录中文件内容的机会。

另外需要注意的是权限并不是给用户运行某个特定程序的权限，而是给用户调用特定 *system call* （来自 Unix API）的权限。比如，像 `cat` 和 `more` 这样的命令是由 `read()` 这一 *system call* 写出来的，故只有当用户拥有对某一文件或目录的 *read* 权限时才能调用这个 *system call*。这也解释了用户无法在他们没有 *read* 权限的目录或文件上使用诸如 `vi` 或 `more` 的原因。相似地，用户也必须拥有对某一文件或目录的 *write* 权限才可以在该文件或目录上调用 `write()` 。*execute* 权限允许用户 `exec()` 一个文件，这也意味着该文件是一个程序（没错，在 Unix 里程序不过就是一个有 *execute* 权限的文件而已）。

简而言之任何程序都会调用一个或多个 system call 来访问文件和目录。一个用户进程必须被授予合适的权限（一个或多个 `r` 来 *read*，`w` 来 *write*，`x` 来 *execute*）。另外，其它的 system call （比如 `stat`）在没有正确的权限的情况下也会失败。

如果你想看看一个程序所调用的所有 system call，可以试试 `strace`（或者其它相似的命令）（这或许会甩给你一啪啦输出）。一旦你知道哪些 system call 被调用，你就可以看看这些 system call 的 *man pages* 来看看该程序需要哪些权限。



# 用户类型

三种基本的权限 `r`, `w`, `x `被应用于三种不同的用户类型。值得注意的是，在 UNIX 中每一个文件和目录都拥有一个所有人 `owner` 和所有组 `group`。三种用户类型分别为所有人 `owner`，所有组 `group` 和 其他 `others` 。

除去那对于 `owner`, `group` 和 `others` 的 `r`, `w` 和 `x` 共计 9种权限模式，其实还有另外三种：The set User ID `SUID` (or `setuid)`, the set Group ID `SGID` (or `setgid`) 以及 The `sticky` bit  (or `text`)。这三比特的实际效果相互依赖，并且对于文件和目录是不一样的。



# 未完待续……



# 结语

可能是被 MySQL 不支持 octal 气昏了头（其实主要是~~自己写不来~~  typeorm 不支持 SQL 自定义函数 ），我看了看我写的那坨翔，竟然越看越顺眼（这TM什么借口）。好吧简单改改就行了，上面写的内容就当学新知识了（光速逃



# 参(chao)考(xi)文献

- [Unix File and Directory Permissions and Modes - by Wayne Pollock, Tampa Florida USA](https://wpollock.com/AUnix1/FilePermissions.htm)
- [Bitwise operators for permissions](https://codereview.stackexchange.com/questions/79020/bitwise-operators-for-permissions)


