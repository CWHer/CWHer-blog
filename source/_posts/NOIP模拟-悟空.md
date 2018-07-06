---
title: '[NOIP模拟]悟空'
date: 2018-07-06 08:56:52
tags: [前缀和]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目描述** 
给出一个n*m的矩阵，矩阵中有A和Q两种字符
求有多少个子矩阵，矩阵的四个角选出三个能构成QAQ

记A为1，Q为0，当且仅当矩阵四个角和为1或2满足条件
剩下的做法和之前的**洛谷P3941 入阵曲**差不多
<!--more-->
```c++
#include<cstdio>
#define LL long long
const int N=505;
int w[N][N],c[N],cnt[N],n,m;
LL ans=0;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
inline char get()
{
    register char ch=getchar();
    while (!(('a'<=ch&&ch<='z')||('A'<=ch&&ch<='Z'))) ch=getchar();
    return ch;
}
int main()
{
    n=read(),m=read();
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
        {
            char opt=get();
            w[i][j]=opt=='A'?1:0;
        }
    for(int i=1;i<=m;++i)
        for(int j=i+1;j<=m;++j)
        {
            for(int k=0;k<=4;k++) cnt[k]=0;
            for(int k=1;k<=n;++k)
            {
                c[k]=w[k][i]+w[k][j];
                ans+=cnt[2-c[k]];
                if (c[k]<=1) ans+=cnt[1-c[k]];
                cnt[c[k]]++;   
            }                      
        }
    printf("%lld",ans);        
    return 0;
}
```

