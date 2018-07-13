---
title: '[NOIP模拟]沙僧'
date: 2018-07-06 09:48:08
tags: [桶]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目描述** 
给出一个n个点的树，其中1为根节点
有m次操作，每次给以a为节点的子树中深度为d的节点+v

用一个桶$$cnt\left [ d \right ]$$记录深度为*d* 的节点要加的值，一遍dfs即可
<!--more-->
```c++
#include<cstdio>
#include<vector>
#define LL long long
using namespace std;
const int N=100050,rt=1;
int n,m,dep[N];
vector<int> e[N],f[N],g[N];
LL w[N],cnt[N];
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
    w[o]+=cnt[dep[o]];
    for(int i=0;i<f[o].size();i++)
        if (f[o][i]==0) w[o]+=g[o][i];
    for(int i=0;i<f[o].size();i++)
        cnt[f[o][i]+dep[o]]+=g[o][i];
    for(int i=0;i<e[o].size();i++)
        if (!dep[e[o][i]])
            dep[e[o][i]]=dep[o]+1,dfs(e[o][i]);
    for(int i=0;i<f[o].size();i++)
        cnt[f[o][i]+dep[o]]-=g[o][i];
}
int main()
{
    n=read(),m=read();
    for(int i=1;i<n;i++)
    {
        int u=read(),v=read();
        e[u].push_back(v);
        e[v].push_back(u);
    }
    for(int i=1;i<=m;i++)
    {
        int x=read(),d=read(),val=read();
        f[x].push_back(d);
        g[x].push_back(val);
    }
    dep[rt]=1,dfs(rt);
    for(int i=1;i<=n;i++) printf("%lld ",w[i]);
    return 0;
}
```


