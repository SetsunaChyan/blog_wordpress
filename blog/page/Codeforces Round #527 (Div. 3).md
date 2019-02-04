[TOC]

## CF1092

[比赛链接](https://codeforces.com/contest/1092 "比赛链接")

[AC代码](https://github.com/SetsunaChyan/OI_source_code "AC代码")(Github)

## A. Uniform String

满足要求的字符串里每个字母的出现次数最多为 $\lfloor \frac{n}{k} \rfloor$ ，剩下的随便输出符合要求的字母就行了。



## B. Teams Forming

最佳方案一定是能力值相邻的两个人组队。

排序一遍然后求 $\sum_{2|i}^n a_{i+1}-a_i$ 。



## C. Prefixes and Suffixes

可以先找出长度为 $n-1$ 的两个子串，可以得到一个合法的原字符串（$n>2$ 时唯一）。

注意前缀串的数量和后缀串的数量要求一致，我们可以先找一遍只能是两者之一的串，然后打一下标记，在分配一下剩下的两者都可以的串。



## D1. Great Vova Wall (Version 1)

先求出墙的最高处 $m$ ，显然如果能够填满，那么最终高度为 $m$ 和 $m+1$ 的情况必能满足至少其一。

然后给这个 $m * n$ 或者 $(m+1) * n$ 的看做一个国际象棋棋盘染色，统计没有放砖头的地方的黑格数与白格数。

如果黑格数与白格数相等那么就是有解，两个棋盘都不满足就无解。



## D2. Great Vova Wall (Version 2)

很容易想到用单调队列维护墙高度递减，每次有更高的墙入队时把低墙的宽度累计进高墙的宽度，如果低墙的宽度不是偶数那么就一定无解。

注意判断一下最后递减的墙是否都是偶数宽，可以考虑最后把高度为 $INF$ 的墙入队，方便统计。

~~这题比D1还简单~~



## E. Minimal Diameter Forest

显然最优解一定是连树的直径的中心。

先考虑找树的直径的中心，两次 $dfs$ 就能找。

再看合并次序，考虑直径分别为 $d1$ 和 $d2$ 的两棵树合并， 那么新直径 $d$ 满足 $ d=max\\{ \lfloor \frac{d1}{2} \rfloor + \lfloor \frac{d2}{2} \rfloor +2,d1,d2 \\} $。

观察这个式子发现 $d1​$ 和 $d2​$ 相差很大时，$d=max\\{ d1,d2 \\}​$ ，所以每次拿直径最小的和最大的合并。然而事实上任意树直接和直径最大的合并就行了，并不要求最小，因为无论如何合并，最大的始终是最大的，而最大的有上限（即答案）。

这个证明并不严格~~基本靠脑补~~ 。



## F. Tree with Maximum Cost

算是经典的树形DP了。

先随便选个点为根， $dfs$ 跑一遍记下当前根的答案 $dp$ 和每颗子树权值之和 $sum_k$。

然后再 $dfs$ 一次开始换根，考虑从根 $i$ 换到根 $j$ ：

1. $dp=dp-sum_j$ 
2. $sum_i=sum_i-sum_j$
3. $dp=dp+sum_i$
4. $sum_j=sum_j+sum_i$ 

换完之后用 $dp$ 去更新答案，回溯的时候别忘了把根再换回来。