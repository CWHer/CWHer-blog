---
title: '[NOIP模拟]拆网线'
date: 2018-07-06 08:26:32
tags: [贪心]
categories: [OI]
password:
top: 0
mathjax: false
---
**题目描述** 
企鹅国的网吧们之间由网线互相连接，形成一棵树的结构。现在由于冬天到了，供暖部门缺少燃料，于是他们决定去拆一些网线来做燃料。但是现在有 K 只企鹅要上网和别人联机游戏，所以他们需要把这 K 只企鹅安排到不同的机房（两只企鹅在同一个机房会吵架），然后拆掉一些网线，但是需要保证每只企鹅至少还能通过留下来的网线和至少另一只企鹅联机游戏。
所以他们想知道，最少需要保留多少根网线？

从叶子节点往上贪心
<!--more-->
```c++
#include<cstdio>
#include<queue>
#include<vector>
using namespace std;
const int N=100050;
vector<int> e[N];
queue<int> Q;
int used[N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void dfs(int o,int fa)
{
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (to==fa) continue;
        dfs(to,o);
    }
    Q.push(o);
}
int main()
{
    int T=read();
    while (T--)
    {
        int n=read(),k=read(),ans=0;
        for(int i=1;i<=n;i++) e[i].clear(),used[i]=0;
        for(int i=1;i<n;i++)
        {
            int u=read(),v=i+1;
            e[u].push_back(v);
            e[v].push_back(u);
        }
        while (!Q.empty()) Q.pop();
        dfs(1,0);
        while (!Q.empty()&&k>1)
        {
            int x=Q.front();Q.pop();
            if (used[x]) continue;
            for(int i=0;i<e[x].size();i++)
            {
                int to=e[x][i];
                if (used[to]) continue;
                used[x]=used[to]=1;
                ans++,k-=2;
                break;
            }
        }
        printf("%d\n",ans+k);
    }
    return 0;
}
```

