---
title: CodeForces-813C The Tag Game
date: 2018-08-13 07:24:47
tags: [树形结构,博弈]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目大意：**
给出一棵以*1* 为根的树
Alice在*1*，Bob在*x*
两人轮流操作，Bob先走
当两人相遇时游戏结束
Bob希望相遇越晚越好，Alice希望相遇越早越好
问游戏最多能进行几轮


观察到Bob一定在Alice的子树内
因此Alice不停向Bob移动即可

Bob则有两种选择
- 在当前点往深度最深的点走
- 先往上走几步，再往深度最深的点走

考虑枚举终点*y*，记*pre* 为$LCA\left ( x,y \right )$
当且仅当$dep\left [ pre \right ]-dep\left [ rt \right ]>dep\left [ x \right ]-dep\left [ pre \right ]$，Bob可以走到*y*
且终点在*y* 时，最多能进行$2*\left ( dep\left [ y \right ]-dep\left [ rt \right ] \right )$轮
<!--more-->
```c++
#include<cstdio>
#include<vector>
#include<algorithm>
using namespace std;
const int N=200050,rt=1;
int log[N],fa[N][25],dep[N],n,x,ans;
vector<int> e[N];
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
    for(int i=1;i<=log[dep[o]];i++)
        fa[o][i]=fa[fa[o][i-1]][i-1];
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (!dep[to])
        {
            dep[to]=dep[o]+1;
            fa[to][0]=o;
            dfs(to);
        }
    }
}
int LCA(int u,int v)
{
    if (dep[u]<dep[v]) swap(u,v);
    for(int i=0;i<=log[dep[u]-dep[v]];i++)
        if (((dep[u]-dep[v])>>i)&1)
            u=fa[u][i];
    if (u==v) return u;
    for(int i=log[dep[u]];i>=0;i--)
        if (fa[u][i]!=fa[v][i])
            u=fa[u][i],v=fa[v][i];
    return fa[u][0];
}
int main()
{
    n=read(),x=read();
    for(int i=1;i<n;i++)
    {
        int u=read(),v=read();
        e[u].push_back(v);
        e[v].push_back(u);
    }
    for(int i=1;i<=n;i++) 
        log[i]=log[i-1]+(1<<log[i-1]==i);
    dep[rt]=1,dfs(rt);
    for(int i=1;i<=n;i++)
    {
        int pre=LCA(i,x);
        if (dep[pre]-dep[rt]>dep[x]-dep[pre]) ans=max(ans,dep[i]-dep[rt]);
    }
    printf("%d\n",ans*2);
    return 0;
}
```



