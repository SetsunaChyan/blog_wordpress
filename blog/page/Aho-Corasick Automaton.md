[TOC]

寒假和开学实在是太忙了，这次是鸽了小半年的AC自动机。

~~我永远喜欢AC自动姬~~

题目主要以匡神专题里的为主。

---

## 模板

先丢个模板上来，不使用动态开点的原因是多组数据的情况下，删 $trie$ 图比较麻烦，而数组实现只要无脑 $memset$ 就行了。

为了方便这里从下标为 $1$ 开始存。

```cpp
int sz,hd=1,nxt[1000005][26],fail[1000005],id[1000005],n;

void trie_clean()
{
    sz=1;
    memset(nxt,0,sizeof(nxt));
    memset(fail,0,sizeof(fail));
    memset(id,0,sizeof(id));
}

void trie_insert(int head,char s[],int len,int idx)
{
    int p=head;
    for(int i=0;i<len;i++)
    {
        int c=s[i]-'a';
        if(!nxt[p][c]) nxt[p][c]=++sz;
        p=nxt[p][c];
    }
    id[p]+=idx;
}

void acatm_build(int head)
{
    int p,tp;
    queue<int> q;
    q.push(head);
    fail[head]=0;
    while(!q.empty())
    {
        p=q.front();
        q.pop();
        for(int i=0;i<26;i++)
            if(nxt[p][i])
            {
                fail[nxt[p][i]]=p==head?head:nxt[fail[p]][i];
                q.push(nxt[p][i]);
            }
            else
                nxt[p][i]=p==head?head:nxt[fail[p]][i];
    }    
}

int acatm_match(int head,char s[],int len)
{
    int p=head,ret=0;
    for(int i=0;i<len;i++)
    {
        int c=(int)s[i]-'a';
        p=nxt[p][c];
        for(int tp=p;tp;tp=fail[tp])
            if(id[tp]) ret++;
    }
    return ret;
}
```

模板题：

| Problem | Title                | Difficulty | Solution                                                     |
| ------- | -------------------- | ----- | :----------------------------------------------------------- |
| HDU2222 | Keywords Search      | 0          | 模板题                                                       |
| HDU2896 | 病毒侵袭             | 0          | 模板题                                                       |
| HDU3065 | 病毒侵袭持续中       | 0          | 模板题（多组数据）                                           |
| ZOJ3430 | Detect the Virus     | 0          | 需要一些麻烦的预处理                                         |
| ZOJ3228 | Searching the String | 0.5        | 处理不可以重叠的串时,可以贪心地取,记录下上次匹配的位置即可;询问可能有重复,把答案记在 $trie$ 图上即可 |

---

## 与矩阵快速幂...

这类都是套路题。

我们知道邻接矩阵的 $m$ 次幂中的每个元素 $A_{i,j}$ 表示点 $i$ 到点 $j$ 的长度为 $k$ 的路径的方案数。

同样，当我们需要求一个长度为 $m$ 的串的方案数，其中串不包含给定的 $n$ 个字符串，我们可以对AC自动机建完的 $trie$ 图建邻接矩阵，记 $A_{i,j}=\sum ({next}[i][k]==j)$ ，特别地，当点 $i$ 或点 $j$ 是某个不该出现的串的结尾时我们记 $A_{i,j}=A_{j,i}=0$ 。(当然如果求含给定字符串的我们就可以先求不含的再拿所有的去减)

然后跑一次矩阵快速幂，矩阵点 $i$ 对应的行的和就是以点 $i$ 为出发点的串的方案数。

有时候我们需要求长度小于等于 $m$ 的符合要求的串的方案数，我们只需要多建一个结束点，然后所有非结尾点都往这个点连一条单向边再跑矩阵快速幂就行了。

| Problem | Title                | Difficulty | Solution                |
| ------- | -------------------- | ----- | ----------------------- |
| POJ2778 | DNA Sequence         | 0.5        | AC自动机+矩阵快速幂     |
| HDU2243 | 考研路茫茫——单词情结 | 1          | 同上;拿所有的减去不含的 |
| POJ1625 | Censored!            | 1          | 同上                    |

---

## 与动态规划...

因为 $trie$ 图对子串的无后效性，利用AC自动机来DP的题也非常多，当然大多数AC自动上的DP也很套路。

一般分为直接在 $trie$ 图上DP和利用 $trie$ 图性质得到额外信息来DP。

因为DP的状态数和 $trie$ 图上顶点数量成正比，所以有些题会卡内存，不要乱开数组就行。

细节都和 $DAG$ 上DP一样，注意顺序。

| Problem | Title                    | Difficulty | Solution                                                     |
| ------- | ------------------------ | ----- | ------------------------------------------------------------ |
| HDU2825 | Wireless Password        | 1          | 状压DP                                                       |
| HDU2296 | Ring                     | 1          | 设 $dp_{i,j} $表示串长 $i$ 且以 $j$ 结尾的最大值             |
| HDU2457 | DNA repair               | 1          | 设 $dp_{i,j}$ 表示长度为 $i$ 终点为 $j$ 的串和母串最大不含坏基因的相同基因个数 |
| HDU4511 | 小明系列故事——女友的考验 | 1          | 设 $dp_{i,j}$ 表示在点 $i$ 且状态在 $j$ 时的满足条件的最短距离 |
| HDU3341 | Lost's revenge           | 1.5        | 这题开四维长 $40$ 的状态会爆内存,事实上不可能用的满,新开个数组用来把状态映射到一维即可 |
| HDU3247 | Resource Archiver        | 2          | 上文说的第二类,利用AC自动机上的最短路来DP                    |
| ZOJ3494 | BCD Code                 | 2.5        | 丢进AC自动机,然后跑数位DP                                    |
| HDU4758 | Walk Through Squares     | 1          | 状压DP                                                       |

