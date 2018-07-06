---
title: '[NOIP模拟]八戒'
date: 2018-07-06 09:18:36
tags: [动态规划,状态压缩]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目描述** 
给出n个点，m条边
每个点有一个权值w，和衰减速度v，每走1单位的路衰减v
保证最优取法所有点权值均大于0

用$$dp\left [i  \right ]\left [ s \right ]$$记录最小衰减值，其中*i*为当前点位置，*s*为当前状态
$$
dp\left [  j\right ]\left [{s}'  \right ] =min\left \{ dp\left [  i\right ]\left [  s\right ] +d\left [ i,j \right ]*\omega \left [ \sim s \right ]\right \}\left ( i\in s,j\notin s \right )
$$
其中$$d\left [ i \right ]\left [  j\right ]$$为*i*到*j*的距离，可以用*Floyd*处理
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
#define LL long long
using namespace std;
const int N=22,INF=1<<20;
int d[N][N],dp[N][1<<N],w[N],rec[1<<N],c[N],n,m,sum;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void floyd()
{
    for(int k=1;k<=n;k++)
        for(int i=1;i<=n;i++) if (i!=k)
            for(int j=1;j<=n;j++) if (k!=j&&i!=j)
                d[i][j]=min(d[i][j],d[i][k]+d[k][j]); 
}
int main()
{
    int T=read();
    while (T--)
    {
        n=read(),m=read(),sum=0;
        for(int i=1;i<=n;i++) w[i]=read(),c[i]=read();
        for(int i=1;i<=n;i++) sum+=w[i];
        for(int i=1;i<=n;i++)
            for(int j=1;j<=n;j++)
                if (i!=j) d[i][j]=INF;
        for(int i=1;i<=m;i++)
        {
            int u=read(),v=read();
            d[u][v]=d[v][u]=min(d[u][v],read());
        }
        for(int i=1;i<=n;i++)
            for(int s=0;s<(1<<n);s++) dp[i][s]=INF;
        for(int s=0;s<(1<<n);s++) rec[s]=0;
        for(int s=0;s<(1<<n);s++)
            for(int i=1;i<=n;i++)
                if ((s>>(i-1))&1) rec[s]+=c[i];
        floyd(),dp[1][1]=0;
        for(int s=0;s<(1<<n);s++)
            for(int i=1;i<=n;i++) if ((s>>(i-1))&1&&dp[i][s]<sum)
                for(int j=1;j<=n;j++) if (!((s>>(j-1))&1))
                    dp[j][s|(1<<(j-1))]=min(dp[j][s|(1<<(j-1))],dp[i][s]+rec[(1<<n)-1-s]*d[i][j]);
        int ans=-INF;
        for(int i=1;i<=n;i++) ans=max(ans,sum-dp[i][(1<<n)-1]);
        printf("%lld\n",ans);
    } 
    return 0;
}
```

