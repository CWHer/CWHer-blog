---
title: CodeChef Flooring
date: 2018-08-13 21:57:14
tags: [数论,分块]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目大意：**
求
$$
\left ( \sum_{i=1}^{n}i^{4}\left \lfloor \frac{n}{i} \right \rfloor \right )\% M
$$


套路是很常见的分块求解
难点主要是$$i^{4}$$求和，$M$不一定为质数

首先有如下几个恒等式
$$
\sum_{i=1}^{n}i^{2}=\frac{n\left ( n+1 \right )\left ( 2n+1 \right )}{6}
$$
$$
\sum_{i=1}^{n}i^{3}=\left ( \frac{n\left ( n+1 \right )}{2} \right )^{2}
$$
$$
\sum_{i=1}^{n}i^{4}=\frac{n\left ( n+1 \right )\left ( 2n+1 \right )\left ( 3n^{2} +3n-1\right )}{30}
$$
其中平方和公式推导如下
$\left ( n+1 \right )^{3}-n^{3}=3n^{2}+3n+1$
$n^{3}-\left ( n-1 \right )^{3}=3\left ( n-1 \right )^{2}+3\left ( n-1 \right )+1$
$\cdots$
累加得
$$
\left ( n+1 \right )^{3}-1^{3}=3*\sum_{i=1}^{n}i^{2}+3*\sum_{i=1}^{n}+n
$$
化简可得
$$
\sum_{i=1}^{n}i^{2}=\frac{n\left ( n+1 \right )\left ( 2n+1 \right )}{6}
$$
立方和，四次方和同理可得

然后是$M$不为质数的问题
有如下公式
$$
\frac{a}{b}\%c=\frac{a\%\left (bc  \right )}{b}
$$
~~然而我以前一直都不知道~~
<!--more-->
```c++
#include<cstdio>
#include<cmath>
#define LL long long
using namespace std;
LL n,mod;
LL sqr(LL x){return x*x%mod;}
LL gcd(LL a,LL b){return b?gcd(b,a%b):a;}
LL calc(LL k)
{
    k%=mod*30;
    LL ret=k*(k+1)%(mod*30);
    ret=ret*(2LL*k+1)%(mod*30);
    ret=ret*(3LL*k*k%(mod*30)+3LL*k%(mod*30)-1);
    return ret%(mod*30)/30;
}
int main()
{
    LL T;
    scanf("%lld",&T);
    while (T--)
    {
        scanf("%lld%lld",&n,&mod);
        LL sz=sqrt(n),ans=0;
        for(LL i=1;n/i>sz;i++)
            ans=(ans+sqr(sqr(i))*(n/i)%mod)%mod;
        for(LL i=sz;i>0;i--)
        {
            LL L=n/(i+1)+1,R=n/i;
            LL val=(calc(R)-calc(L-1)+mod)%mod;
            ans=(ans+val*i%mod)%mod;
        }
        printf("%lld\n",(ans%mod+mod)%mod);
    }
    return 0;
}
```

