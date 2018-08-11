---
title: '[USACO07OPEN]城市的地平线'
date: 2018-08-11 16:16:07
tags: [线段树,扫描线]
categories: [OI]
password:
top: 0
mathjax: false
---
竖版的扫描线
扫描线参考[HDU1542 Atlantis](https://cwher.github.io/2018/08/11/HDU1542-Atlantis)
<!--more-->

```c++
#include<cstdio>
#include<algorithm>
#define LL long long
using namespace std;
const int N=80050;
struct node{int x,h,val;}t[N];
int n,cnt=0,ql,qr,w[N];
LL ans=0,sz[N<<4],num[N<<4];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while ((ch<'0'||ch>'9')&&ch!='-') ch=getchar();
    if (ch=='-') {t=-1;ch=getchar();}
    while (ch>='0'&&ch<='9') {x=x*10+ch-48;ch=getchar();}
    return x*t;
}
void calc(int o,int l,int r)
{
    if (num[o])
        sz[o]=w[r+1]-w[l];
    else
        sz[o]=sz[o<<1]+sz[o<<1|1];
}
void modify(int o,int l,int r,int x)
{
    if (ql<=l&&r<=qr)
    {
        num[o]+=x;
        calc(o,l,r);
        return;
    }
    int mid=(l+r)>>1;
    if (ql<=mid) modify(o<<1,l,mid,x);
    if (qr>mid) modify(o<<1|1,mid+1,r,x);
    calc(o,l,r);
}
bool cmp(node a,node b){return a.x<b.x;};
int main()
{
    n=read();
    for(int i=1;i<=n;i++)
    {
        int l=read(),r=read(),h;
        w[i]=h=read();
        t[++cnt]=(node){l,h,1};
        t[++cnt]=(node){r,h,-1};
    }
    w[n+1]=0;
    sort(w+1,w+n+2);
    int idx=unique(w+1,w+n+2)-w-1;
    n<<=1;
    sort(t+1,t+n+1,cmp);
    for(int i=1;i<n;i++)
    {
        ql=1,qr=lower_bound(w+1,w+idx+1,t[i].h)-w-1;
        if (ql<=qr) modify(1,1,n,t[i].val);
        ans+=sz[1]*((LL)t[i+1].x-t[i].x);
    }
    printf("%lld\n",ans);
    return 0;
}
```

