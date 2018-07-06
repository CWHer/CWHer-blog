---
title: '[NOIP模拟]奇怪的队列'
date: 2018-07-06 08:45:57
tags: [线段树,贪心]
categories: [OI]
password:
top: 0
mathjax: false
---
**题目描述** 
nodgd的粉丝太多了，每天都会有很多人排队要签名。 
今天有n个人排队，每个人的身高都是一个整数，且互不相同。很不巧，nodgd今天去忙别的事情去了，就只好让这些粉丝们明天再来。同时nodgd提出了一个要求，每个人都要记住自己前面与多少个比自己高的人，以便于明天恢复到今天的顺序。 
但是，粉丝们或多或少都是有些失望的，失望使她们晕头转向、神魂颠倒，已经分不清楚哪一边是“前面”了，于是她们可能是记住了前面比自己高的人的个数，也可能是记住了后面比自己高的人的个数，而且他们不知道自己记住的是哪一个方向。 
nodgd觉得，即使这样明天也能恢复出一个排队顺序，使得任意一个人的两个方向中至少有一个方向上的比他高的人数和他记住的数字相同。可惜n比较大，显然需要写个程序来解决，nodgd很忙，写程序这种事情就交给你了。

从小到大考虑每个人，对于每个人贪心地考虑位置
用线段树维护，支持求排名和单点修改
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
const int N=100050,INF=N<<2;
struct node {int val,w;} t[N];
int n,c[N],sz[N<<2];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void build (int o,int l,int r)
{
    if (l==r) {sz[o]=1;return;}
    int mid=(l+r)>>1;
    build(o<<1,l,mid);
    build(o<<1|1,mid+1,r);
    sz[o]=sz[o<<1]+sz[o<<1|1];
}
void add(int o,int l,int r,int k,int x)
{
    if (l==r) {sz[o]+=x;return;}
    int mid=(l+r)>>1;
    if (k<=mid)
        add(o<<1,l,mid,k,x);
    else
        add(o<<1|1,mid+1,r,k,x);
    sz[o]=sz[o<<1]+sz[o<<1|1];
}
int ask_pre(int o,int l,int r,int k)
{
    if (l==r) return r;
    int mid=(l+r)>>1;
    if (sz[o<<1]>=k)
        return ask_pre(o<<1,l,mid,k);
    else
        return ask_pre(o<<1|1,mid+1,r,k-sz[o<<1]);
}
int ask_nxt(int o,int l,int r,int k)
{
    if (l==r) return r;
    int mid=(l+r)>>1;
    if (sz[o<<1|1]>=k)
        return ask_nxt(o<<1|1,mid+1,r,k);
    else
        return ask_nxt(o<<1,l,mid,k-sz[o<<1|1]);
}
bool cmp(node a,node b) {return a.val<b.val;}
int main()
{
    n=read();
    for(int i=1;i<=n;i++) t[i].val=read(),t[i].w=read();
    sort(t+1,t+n+1,cmp);
    build(1,1,n);
    for(int i=1;i<=n;i++)
    {
        int pos=INF;
        if (sz[1]<=t[i].w) {puts("impossible");return 0;}
        pos=min(pos,ask_pre(1,1,n,t[i].w+1));
        pos=min(pos,ask_nxt(1,1,n,t[i].w+1));
        c[pos]=t[i].val;
        add(1,1,n,pos,-1);
    }
    for(int i=1;i<=n;i++) printf("%d ",c[i]);
    return 0;
}
```


