---
title: POJ2411 Mondriaan's Dream
date: 2018-07-22 20:09:20
tags: [DP,状态压缩,轮廓线]
categories: [OI]
password:
top: 0
mathjax: true
---
经典的状压DP题，解法很多
这里讲下状压轮廓线的做法
![Mtrx](POJ2411-Mondriaan-s-Dream/mtrx.jpg)
*s* 从大到小保存
若$$s_{5}=0$$，必须要竖放一个
否则可以横放也可以不放
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
#define LL long long
using namespace std;
const int N=18;
int n,m;
LL dp[2][1<<N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
int main()
{
    while (n=read(),m=read())
    {
        if (n<m) swap(n,m);
        for(int s=0;s<(1<<m);s++) dp[0][s]=0;
        dp[0][(1<<m)-1]=1;
        for(int i=1,now=1;i<=n;i++)
            for(int j=1;j<=m;j++,now^=1)
            {
                for(int s=0;s<(1<<m);s++) dp[now][s]=0;
                for(int s=0;s<(1<<m);s++)
                    if (!(s>>(m-1))&1)
                        dp[now][(s<<1)|1]+=dp[now^1][s];
                    else
                    {
                        dp[now][(s<<1)&((1<<m)-1)]+=dp[now^1][s];
                        if (j!=1&&!(s&1)) dp[now][((s<<1)|3)&((1<<m)-1)]+=dp[now^1][s];
                    }
            }    
        printf("%lld\n",dp[(n*m)&1][(1<<m)-1]);
    }
    return 0;
}
```

