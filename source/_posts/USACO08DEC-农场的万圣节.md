---
title: '[USACO08DEC]农场的万圣节'
date: 2018-08-11 14:34:25
tags: [缩点,图论]
categories: [OI]
password:
top: 0
mathjax: false
---
缩完点之后，求一遍每个点到终点的路径长即可
<!--more-->
```c++
#include<cstdio>
#include<vector>
#include<stack>
using namespace std;
const int N=100050;
struct node{int sz;}t[N];
int n,idx[N],cnt=0,sz[N],used[N];
int dfn[N],low[N],num=0,inq[N];
vector<int> e[N],f[N];
stack<int> S;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void tarjan(int o)
{
    dfn[o]=low[o]=++num;
    inq[o]=1,S.push(o);
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (!dfn[to])
        {
            tarjan(to);
            low[o]=min(low[o],low[to]);
        }
        else if (inq[to])
            low[o]=min(low[o],dfn[to]);
    }
    if (low[o]==dfn[o])
    {
        int x;cnt++;
        do{
            x=S.top();
            idx[x]=cnt;
            S.pop(),inq[x]=0;
            t[cnt].sz++;
        }while (x!=o);
    }
}
void dfs(int o)
{
    if (used[o]) return;
    used[o]=1;
    sz[o]=t[o].sz;
    for(int i=0;i<f[o].size();i++)
    {
        int to=f[o][i];
        dfs(to);
        sz[o]+=sz[to];
    }
}
int main()
{
    n=read();
    for(int u=1;u<=n;u++)
    {
        int v=read();
        e[u].push_back(v);
    }
    for(int i=1;i<=n;i++)
        if (!dfn[i]) tarjan(i);
    for(int u=1;u<=n;u++)
        for(int i=0;i<e[u].size();i++)
        {
            int v=e[u][i];
            if (idx[u]!=idx[v]) 
                f[idx[u]].push_back(idx[v]);
        }
    for(int i=1;i<=cnt;i++)
        if (!used[i]) dfs(i);
    for(int i=1;i<=n;i++)
        printf("%d\n",sz[idx[i]]);
    return 0;
}
```

