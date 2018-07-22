---
title: '[POI2011]ROT-Tree Rotations'
date: 2018-06-13 22:31:51
tags: [线段树]
categories: [OI]
password:
top: 0
mathjax: false
---
对于每个节点分别求左-右，右-左的逆序对，将较小的加入答案
原来思路：枚举个数较少的节点的每个数，二分求答案
看完题解发现可以用线段树合并。新技能*get*
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
#define LL long long
using namespace std;
const int N=400050;
int cnt,st,ch[N][2],w[N],rt[N],n;
int idx,ls[N*20],rs[N*20],sum[N*20];
LL ans,f,g;
void dfs(int &o)
{
    o=++cnt;
    scanf("%d",&w[o]);
    if (!w[o]) dfs(ch[o][0]),dfs(ch[o][1]);
}
void build(int &o,int l,int r,int k)
{
    o=++idx;
    if (l==r) {sum[o]=1;return;}
    int mid=(l+r)>>1;
    k<=mid?build(ls[o],l,mid,k):build(rs[o],mid+1,r,k);
    sum[o]=sum[ls[o]]+sum[rs[o]];
}
int merge(int u,int v)
{
    if (!u||!v) return v+u;
    f+=(LL)sum[rs[u]]*sum[ls[v]];
    g+=(LL)sum[ls[u]]*sum[rs[v]];
    int o=++idx;
    ls[o]=merge(ls[u],ls[v]);
    rs[o]=merge(rs[u],rs[v]);
    sum[o]=sum[ls[o]]+sum[rs[o]];
    return o;
}
void calc(int o)
{
    if (!o) return;
    calc(ch[o][0]),calc(ch[o][1]);
    if (!w[o]) 
    {
        f=g=0;
        rt[o]=merge(rt[ch[o][0]],rt[ch[o][1]]);
        ans+=min(f,g);	
    }
}
int main()
{
    scanf("%d",&n);
    dfs(st);
    for(int i=1;i<=cnt;i++)
        if (w[i]) build(rt[i],1,n,w[i]);
    calc(st);
    printf("%lld",ans);
    return 0;
}
```

