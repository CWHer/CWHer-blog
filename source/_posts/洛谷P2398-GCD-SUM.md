---
title: '洛谷P2398 GCD SUM'
date: 2018-07-15 18:52:09
tags: [数论]
categories: [OI]
password:
top: 0
mathjax: true
---
直接计算复杂度过高无法接受
考虑枚举所有$$gcd$$值计算
若$$gcd=k$$
对于所有$$gcd\left ( x,y \right )=1$$，有$$gcd\left ( xk,yk \right )=k\left ( xk\leq n,yk\leq n \right )$$
所以$$gcd=k$$的个数为
$$
2\sum_{i=1}^{\left \lfloor \frac{n}{k} \right \rfloor}\varphi \left ( i \right )-1
$$
复杂度$$O\left (N \right )$$


还有一种更妙的做法
设$$f\left [ k \right ]$$为$$gcd=k$$的个数
设$$g\left [ k \right ]$$为$$k|gcd$$的个数
$$
g\left [ k \right ]=\sum_{i=1}^{\left \lfloor \frac{n}{k} \right \rfloor}f\left [ ik \right ]=\left [ \frac{n}{k} \right ]^{2}
$$
$$
f\left [ k \right ]=g\left [ k \right ]-\sum_{i=2}^{\left \lfloor \frac{n}{k} \right \rfloor}f\left [ ik \right ]
$$
倒序计算即可
复杂度$$O\left ( NlogN \right )$$
<!--more-->
```c++
#include<cstdio>
#define LL long long
const int N=100050;
int prime[N],cnt=0,phi[N];
LL n,sum[N],ans=0;
void init()
{
    phi[1]=1;
    for(int i=2;i<=n;i++)
    {
        if (!phi[i]) prime[++cnt]=i,phi[i]=i-1;
        for(int j=1;j<=cnt;j++)
        {
            if (prime[j]*i>n) break;
            if (i%prime[j]==0)
            {
                phi[i*prime[j]]=phi[i]*prime[j];
                break;
            }
            else phi[i*prime[j]]=phi[i]*(prime[j]-1);
        }		
    }
    for(int i=1;i<=n;i++) sum[i]=sum[i-1]+phi[i];
}
int main()
{
    scanf("%lld",&n);
    init();
    for(LL i=1;i<=n;i++) 
        ans+=(sum[n/i]*2-1)*i;
    printf("%lld",ans);
    return 0;
}
```

```c++
#include<cstdio>
#define LL long long
const int N=100050;
LL n,f[N],g[N],ans=0;
int main()
{
    scanf("%lld",&n);
    for(LL i=1;i<=n;i++) g[i]=(n/i)*(n/i);
    for(LL i=n;i>0;i--) 
    {
    	f[i]=g[i];
    	for(LL j=2;i*j<=n;j++) f[i]-=f[i*j];
    	ans+=f[i]*i; 	
	}
    printf("%lld",ans);
    return 0;
}
```

