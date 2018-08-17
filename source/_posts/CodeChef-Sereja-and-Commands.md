---
title: CodeChef Sereja and Commands
date: 2018-08-14 13:33:26
tags: [线段树,差分]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目大意：**
给出一个*n* 个元素的序列，初始值均为*0*
有两种操作
- 将$\left [ l,r \right ]$的元素+1
- 执行$\left [ l,r \right ]$的所有操作，保证$r$小于当前操作编号

问执行完所有操作后序列中每个元素的值


直接按照操作给出顺序执行会产生递归，无法有效处理
考虑倒着执行操作，这样就没有递归问题了
用一个支持单点查询，区间修改的数据结构维护每个操作执行次数就好了
由于只在操作完询问一次序列中的值
因此序列中的操作用差分即可

~~本来换行写的是$puts$，不知道为什么$RE$掉了~~
<!--more-->
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
#define LL long long
using namespace std;
const int N=1e5+50,mod=1e9+7;
struct node{int opt,l,r;}q[N];
int T,n,m,ql,qr;
LL c[N<<2],t[N<<2],w[N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void add(int o,int l,int r,LL x)
{
    if (ql<=l&&r<=qr)
    {
        c[o]=(c[o]+x*(r-l+1))%mod;
        t[o]=(t[o]+x)%mod;
        return;
    }
    int mid=(l+r)>>1;
    if (ql<=mid) add(o<<1,l,mid,x);
    if (qr>mid) add(o<<1|1,mid+1,r,x);
    c[o]=(c[o]+(min(r,qr)-max(l,ql)+1)*x)%mod;
}
LL ask(int o,int l,int r)
{
    if (ql<=l&&r<=qr) return c[o];
    int mid=(l+r)>>1;LL ret=0;
    if (ql<=mid) ret=(ret+ask(o<<1,l,mid))%mod;
    if (qr>mid) ret=(ret+ask(o<<1|1,mid+1,r))%mod;
    return (ret+(min(r,qr)-max(l,ql)+1)*t[o])%mod;
}
int main()
{
    int T=read();
    while (T--)
    {
        n=read(),m=read();
        memset(c,0,sizeof(c));
        memset(t,0,sizeof(t));
        memset(w,0,sizeof(w));
        for(int i=1;i<=m;i++)
        {
            q[i].opt=read();
            q[i].l=read();
            q[i].r=read();
        }
        for(int i=m;i>0;i--)
        {
            ql=qr=i;
            LL x=(ask(1,1,m)+1)%mod;
            if (q[i].opt==1)
            {
                w[q[i].l]=(w[q[i].l]+x)%mod;
                w[q[i].r+1]=(w[q[i].r+1]-x)%mod;
            }
            else
            {
                ql=q[i].l,qr=q[i].r;
                add(1,1,m,x);
            }
        }
        for(int i=1;i<=n;i++) w[i]=(w[i]+w[i-1])%mod;
        for(int i=1;i<=n;i++) printf("%lld ",(w[i]+mod)%mod);
        printf("\n");
    }
    return 0;
}
```

