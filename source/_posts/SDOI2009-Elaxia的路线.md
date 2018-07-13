---
title: '[SDOI2009]Elaxia的路线'
date: 2018-07-10 19:31:31
tags: [最短路]
categories: [OI]
password:
top: 0
mathjax: true
---
记*E* 为Elaxia 的最短路DAG，*W* 为w 的最短路DAG
根据*W* 重构*E* 每条边的权值
对于连接u，v，权值为val的边
当且仅当在*W* 中$$d\left [ u \right ]+val= d\left [ v \right ]$$或$$d\left [ v \right ]+val= d\left [ u\right ]$$时，边权为val，否则为0
在*E* 上跑一边最长路得出答案
<!--more-->
```c++
#include<cstdio>
#include<vector>
#include<queue>
using namespace std;
const int N=2000,INF=1<<25;
int n,m,st[2],ed[2],d[2][N],inq[N],used[N],dp[N];
vector<int> f[N],g[N],w[2][N],e[2][N],c[N];
queue<int> Q;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while ((ch<'0'||ch>'9')&&ch!='-') ch=getchar();
    if (ch=='-') {t=-1;ch=getchar();}
    while (ch>='0'&&ch<='9') {x=x*10+ch-48;ch=getchar();}
    return x*t;
}
void SPFA(int s,int k)
{
    for(int i=1;i<=n;i++) d[k][i]=INF,inq[i]=0;
    d[k][s]=0,inq[s]=1,Q.push(s);
    while (!Q.empty())
    {
        int x=Q.front();
        Q.pop(),inq[x]=0;
        for(int i=0;i<f[x].size();i++)
        {
            int to=f[x][i],val=g[x][i];
            if (d[k][x]+val==d[k][to]) 
                e[k][to].push_back(x),w[k][to].push_back(val);
            if (d[k][x]+val<d[k][to])
            {
                e[k][to].clear();
                e[k][to].push_back(x);
                w[k][to].clear();
                w[k][to].push_back(val);
                d[k][to]=d[k][x]+val;
                if (!inq[to]) Q.push(to),inq[to]=1;
            }
        }
    }
}
void add(int u,int v,int val)
{
    g[u].push_back(val);
    f[u].push_back(v);
}
void dfs(int o,int x)
{
    if (used[o]) return;
    used[o]=1;
    if (o==x) return;		
    for(int i=0;i<e[1][o].size();i++) dfs(e[1][o][i],x);	
}
int calc(int o,int x)
{
    if (dp[o]!=-1) return dp[o];
    if (o==x) return dp[o]=0;
    for(int i=0;i<e[0][o].size();i++)
        dp[o]=max(dp[o],calc(e[0][o][i],x)+c[o][i]);
    return dp[o];	
}
int main()
{
    n=read(),m=read();
    for(int i=0;i<2;i++) st[i]=read(),ed[i]=read();
    for(int i=1;i<=m;i++)
    {
        int u=read(),v=read(),val=read();
        add(u,v,val),add(v,u,val);
    }
    for(int i=0;i<2;i++) SPFA(st[i],i);	
    dfs(ed[1],st[1]);
    for(int i=1;i<=n;i++)
        if (!used[i]) d[1][i]=INF;
    for(int u=1;u<=n;u++)
        for(int i=0;i<e[0][u].size();i++)
        {
            int v=e[0][u][i],f=0;
            if (d[1][u]+w[0][u][i]==d[1][v]) f++;
            if (d[1][v]+w[0][u][i]==d[1][u]) f++;
            c[u].push_back(f*w[0][u][i]);
        }
    for(int i=1;i<=n;i++) dp[i]=-1;
    printf("%d",calc(ed[0],st[0]));
    return 0;
}
```

