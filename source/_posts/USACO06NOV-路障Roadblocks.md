---
title: '[USACO06NOV]路障Roadblocks'
date: 2018-07-19 20:14:10
tags: [最短路]
categories: [OI]
password:
top: 0
mathjax: true
---
和次小生成树相似的套路
记*f* 为*S* 到所有点的最短路，*g* 为*T* 到所有点的最短路
枚举所有边$$edge\left \{ u,v,val \right \}$$
$$ans=min\left \{ f\left [ u \right ]+val+ g\left [ v \right ]\right \}\left (  f\left [ u \right ]+val+ g\left [ v \right ]>f\left [ T \right ] \right )$$
<!--more-->
```c++
#include<cstdio>
#include<queue>
using namespace std;
const int N=5050,INF=1<<30;
int n,m,d[2][N],inq[N],mins=INF,ans=INF;
vector<int> e[N],g[N];
queue<int> Q;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void add(int u,int v,int val)
{
    e[u].push_back(v);
    g[u].push_back(val);
}
void SPFA(int s,int k)
{
    for(int i=1;i<=n;i++) d[k][i]=INF;
    d[k][s]=0,inq[s]=1,Q.push(s);
    while (!Q.empty())
    {
        int x=Q.front();
        inq[x]=0,Q.pop();
        for(int i=0;i<e[x].size();i++)
        {
            int to=e[x][i];
            if (d[k][x]+g[x][i]<d[k][to])
            {
                d[k][to]=d[k][x]+g[x][i];
                if (!inq[to]) inq[to]=1,Q.push(to);
            }
        }
    }
}
int main()
{
    n=read(),m=read();
    for(int i=1;i<=m;i++)
    {
        int u=read(),v=read();
        int val=read();
        add(u,v,val);
        add(v,u,val);
    }
    SPFA(1,0),SPFA(n,1);
    mins=d[0][n];
    for(int i=1;i<=n;i++)
        for(int j=0;j<e[i].size();j++)
        {
            int u=i,v=e[i][j];
            if (d[0][u]+g[i][j]+d[1][v]>mins)
                ans=min(ans,d[0][u]+g[i][j]+d[1][v]);
        }
    printf("%d\n",ans);
    return 0;
}
```

