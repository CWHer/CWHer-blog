---
title: Lucas定理
date: 2018-07-26 20:44:36
tags: [组合数学,模板]
categories: [OI]
password:
top: 0
mathjax: true
---
~~证明省略~~
$$
Lucas\left ( m,n,p \right )=C\left ( m\%p,n\%p \right )*Lucas\left ( \left \lfloor \frac{m}{p}\right  \rfloor ,\left \lfloor \frac{n}{p} \right \rfloor ,p \right )
$$
$$
Lucas\left ( m,0,p \right )=1
$$
<!--more-->

```c++
#include<cstdio>
#define LL long long
const int N=200050;
LL fact[N];
int n,m,mod;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void init()
{
    fact[0]=1;
    for(int i=1;i<=n+m;i++) 
        fact[i]=fact[i-1]*i%mod;
}
LL pow(LL a,LL b,LL mod)
{
    LL ret=1;
    for(;b;b>>=1)
    {
        if (b&1) ret=ret*a%mod;
        a=a*a%mod;
    }
    return ret;
}
LL C(LL m,LL n,LL mod)
{
    if (m<n) return 0;
    return fact[m]*pow(fact[n]*fact[m-n]%mod,mod-2,mod)%mod;
}
LL Lucas(LL m,LL n,LL mod)
{
    if (!n) return 1;
    return Lucas(m/mod,n/mod,mod)*C(m%mod,n%mod,mod)%mod;
}
int main()
{
    int T=read();
    while (T--)
    {
        n=read(),m=read();
        mod=read();
        init();
        printf("%lld\n",Lucas(n+m,m,mod));
    }
    return 0;
}
```

