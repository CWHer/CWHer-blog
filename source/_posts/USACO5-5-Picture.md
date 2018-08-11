---
title: '[USACO5.5]Picture'
date: 2018-08-11 16:26:49
tags: [扫描线,线段树]
categories: [OI]
password:
top: 0
mathjax: true
---
扫描线参考[HDU1542 Atlantis](https://cwher.github.io/2018/08/11/HDU1542-Atlantis)
可以横竖各做一遍
需要注意的是会重复计算，需要与上一次结果作差


也可以只做一遍
用$$sz$$数组记录宽度，$$c$$数组记录竖线数量
还需要$$L$$，$$R$$记录是否有左右端点竖线
需要注意的是端点竖线重合和顺序问题
高度相同时先做覆盖再做取消覆盖，否则会重叠线段会多次计算
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
const int N=2e4+50;
struct node{int l,r,h,val;}t[N<<2];
int n,cnt=0,ans=0,ql,qr;
int num[N<<2],sz[N<<2],w[N<<2];
int c[N<<2],L[N<<2],R[N<<2];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while ((ch<'0'||ch>'9')&&ch!='-') ch=getchar();
    if (ch=='-') {t=-1;ch=getchar();}
    while (ch>='0'&&ch<='9') {x=x*10+ch-48;ch=getchar();}
    return x*t;
}
bool cmp(node a,node b)
{
    return a.h==b.h?a.val>b.val:a.h<b.h;
};
void calc(int o,int l,int r)
{
    if (num[o])
    {
        sz[o]=w[r+1]-w[l];
        c[o]=(L[o]=1)+(R[o]=1);
    }
    else
    {
        L[o]=L[o<<1],R[o]=R[o<<1|1];
        c[o]=c[o<<1]+c[o<<1|1];
        c[o]-=(R[o<<1]&L[o<<1|1])<<1;
        sz[o]=sz[o<<1]+sz[o<<1|1];
    }
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
int main()
{
    n=read();
    for(int i=1;i<=n;i++)
    {
        int x1=read(),y1=read();
        int x2=read(),y2=read();
        w[i]=x1,w[i+n]=x2;
        t[++cnt]=(node){x1,x2,y1,1};
        t[++cnt]=(node){x1,x2,y2,-1};
    }
    n<<=1;
    sort(w+1,w+n+1);
    int idx=unique(w+1,w+n+1)-w-1;
    sort(t+1,t+n+1,cmp);
    for(int pre=0,i=1;i<=n;i++)
    {
        ql=lower_bound(w+1,w+idx+1,t[i].l)-w;
        qr=lower_bound(w+1,w+idx+1,t[i].r)-w-1;
        if (ql<=qr) modify(1,1,idx,t[i].val);
        ans+=c[1]*(t[i+1].h-t[i].h);
        ans+=abs(sz[1]-pre),pre=sz[1];
    }
    printf("%d\n",ans);
    return 0;
}
```

