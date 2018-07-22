---
title: '[USACO07NOV]奶牛接力Cow Relays'
date: 2018-07-19 19:19:20
tags: [图论,DP,矩阵加速]
categories: [OI]
password:
top: 0
mathjax: true
---
记$$dp\left [ t \right ]\left [ i \right ]\left [  j\right ]$$为*i* 到*j* 经过*t* 条边的最短路
转移方程为
$$
dp\left [ t+1 \right ]\left [ i \right ]\left [  j\right ]=min\left \{ dp\left [ t \right ]\left [ i \right ]\left [ k \right ]+ d\left [ k \right ]\left [ j \right ]\right \}\left ( 1\leq k\leq n \right )
$$
~~通过观察~~，发现很像矩阵乘法，然后还满足交换律
因此
$$
dp\left [ t \right ]=dp\left [ 0 \right ]*D^{t}
$$
$$dp\left [ 0 \right ]$$矩阵中除$$dp\left [ 0 \right ]\left [ st \right ]\left [ st \right ]= 0$$，其它为*INF*
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
#define LL long long
using namespace std;
const int N=205;
const LL INF=1LL<<60;
int pos[N<<10],cnt=0,n,m,st,ed;;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
namespace Matrix
{
    struct mtrx{LL w[N][N];} d;
    void fill(mtrx &M,LL x)
    {
        for(int i=1;i<=cnt;i++)
            for(int j=1;j<=cnt;j++) M.w[i][j]=x;
    }
    void cpy(mtrx &M,mtrx &C)
    {
        for(int i=1;i<=cnt;i++)
            for(int j=1;j<=cnt;j++) 
                M.w[i][j]=C.w[i][j];
    }
    void mul(mtrx &M,mtrx C)
    {
        mtrx ret;
        fill(ret,INF);
        for(int i=1;i<=cnt;i++)
            for(int j=1;j<=cnt;j++)
                for(int k=1;k<=cnt;k++)
                    ret.w[i][j]=min(ret.w[i][j],M.w[i][k]+C.w[k][j]);
        cpy(M,ret);
    }
    void pow(mtrx &M,int x)
    {
        mtrx ret;
        fill(ret,INF);
        ret.w[pos[st]][pos[st]]=0;
        for(;x;x>>=1)
        {
            if (x&1) mul(ret,M);
            mul(M,M);
        }
        cpy(M,ret);
    }
};
using namespace Matrix;
int main()
{
    n=read(),m=read(),st=read(),ed=read();
    for(int i=1;i<N;i++)
        for(int j=1;j<N;j++) d.w[i][j]=INF;
    for(int i=1;i<=m;i++)
    {
        LL val=read();
        int u=read(),v=read();
        if (!pos[u]) pos[u]=++cnt;
        if (!pos[v]) pos[v]=++cnt;
        u=pos[u],v=pos[v];
        d.w[u][v]=d.w[v][u]=min(d.w[u][v],val);
    }
    pow(d,n);
    printf("%lld\n",d.w[pos[st]][pos[ed]]);
    return 0;
}
```

