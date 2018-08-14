---
title: CodeForces-734E Anton and Tree
date: 2018-08-12 19:18:35
tags: [树形结构,直径]
categories: [OI]
password:
top: 0
mathjax: false
---
**题目大意： **
给出一棵黑白两色的树
每次操作可以改变一个同色联通块的颜色
问最少需要几次操作


首先缩个点，显然每次操作改变整个同色联通块更优
然后求直径，答案为$$\left \lceil \frac{len}{2} \right \rceil$$
因为在缩直径时，其它枝叶也会缩掉，这个感性理解即可
<!--more-->
```c++
#include<cstdio>
#include<vector>
using namespace std;
const int N=200050;
int col[N],n,idx[N],d[N],st,ed,cnt=0;
vector<int> e[N],f[N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void dfs(int o)
{
    idx[o]=cnt;
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (!idx[to]&&col[o]==col[to]) dfs(to);
    }
}
void find(int o,int pre,int &x)
{
    if (o==pre) d[o]=0;
    if (!x||d[o]>d[x]) x=o;
    for(int i=0;i<f[o].size();i++)
    {
        int to=f[o][i];
        if (to!=pre)
        {
            d[to]=d[o]+1;
            find(to,o,x);
        }
    }
}
int main()
{
    n=read();
    for(int i=1;i<=n;i++) col[i]=read();
    for(int i=1;i<n;i++)
    {
        int u=read(),v=read();
        e[u].push_back(v);
        e[v].push_back(u);
    }
    for(int i=1;i<=n;i++)
        if (!idx[i]) cnt++,dfs(i);
    for(int u=1;u<=n;u++)
        for(int i=0;i<e[u].size();i++)
        {
            int v=e[u][i];
            if (idx[u]!=idx[v])
                f[idx[u]].push_back(idx[v]);
        }
    find(cnt,cnt,ed);
    find(ed,ed,st);
    printf("%d\n",(d[st]+1)/2);
    return 0;
}
```

