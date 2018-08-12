---
title: CodeForces-739B
date: 2018-08-12 14:54:52
tags: [树形结构]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目大意: **
给出一棵*n* 个节点的树，每条边有一个权值
若*v* 在*u* 的子树内，且$$d_{u-v}\leq a_{v}$$，则称*u* 能控制*v*
问每个点能控制多少点



因为*v* 在*u* 的子树内，所以$$d_{u-v}=dep_{v}-dep_{u}$$
化简得$$dep_{v}-a_{v}\leq dep_{u}$$
观察到$$dep_{v}-a_{v}$$为定值，且$$dep_{u}$$单调递增
找到第一个点*u*，满足$$dep_{v}-a_{v}\leq dep_{u}$$，则$$u-v_{fa}$$路径上所有点均可控制点$$v$$
由于只有一次询问，可用树上差分
至于怎么找第一个满足的点*u*，可以用二分或倍增

感觉倍增实现起来会简单一点，代码用的是二分
<!--more-->
```c++
#include<cstdio>
#include<vector>
#define LL long long
using namespace std;
const int N=200050,rt=1;
int n,w[N],cnt[N],fa[N];
LL dep[N];
vector<int> e[N],g[N],Q;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void add(int u,int v,int val)
{
    e[u].push_back(v);
    g[u].push_back(val);
}
int find(LL key)
{
    if (Q.empty()) return 0;
    if (dep[Q.back()]<key) return Q.back();
    int L=0,R=Q.size()-1;
    while (L<R)
    {
        int mid=(L+R)>>1;
        if (dep[Q[mid]]<key)
            L=mid+1;
        else
            R=mid;
    }
    return Q[R];
}
void dfs(int o,int pre)
{
    if (!Q.empty()&&dep[o]-w[o]<=dep[Q.back()])
    {
        cnt[fa[find(dep[o]-w[o])]]--;
        cnt[fa[o]]++;
    }
    Q.push_back(o); 
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (to!=pre)
        {
            dep[to]=dep[o]+g[o][i];
            fa[to]=o;      
            dfs(to,o);         
        }
    }
    Q.pop_back();
}
void calc(int o)
{
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (dep[to]>dep[o]) 
        {
            calc(to);
            cnt[o]+=cnt[to];
        }
    }
}
int main()
{
    n=read();
    for(int i=1;i<=n;i++) w[i]=read();
    for(int u=2;u<=n;u++)
    {
        int v=read(),val=read();
        add(u,v,val);
        add(v,u,val);
    }
    dfs(rt,0),calc(rt);
    for(int i=1;i<=n;i++) printf("%d ",cnt[i]);
    return 0;
}
```

