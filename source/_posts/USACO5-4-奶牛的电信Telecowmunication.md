---
title: '[USACO5.4]奶牛的电信Telecowmunication'
date: 2018-07-10 19:54:15
tags: [网络流]
categories: [OI]
password:
top: 0
mathjax: true
---
记$$edge\left \{ u,v,capacity,cost \right \}$$为一条从*u* 到*v* 的，流量为*cap* 的，费用为*cost* 的弧与反弧
拆点，对于点*x*，分为*x* 和*x+n* 两点，*x* 负责流入，*x+n* 负责流出
对于每个点连边$$edge\left \{ x,x+n,1 \right \}$$
对于原图每条连接*u,v* 的边，连边$$edge\left \{ u+n,v,INF \right \}$$，$$edge\left \{ v+n,u,INF \right \}$$
跑一边*s+n* 到*t* 的最大流
最大流最小割
<!--more-->
```c++
#include<cstdio>
#include<queue>
using namespace std;
const int M=10000,N=500,INF=1<<25;
int n,m,st,ed,head[N],cnt=0,d[N];
struct edge{int to,next,flow,cap;} e[M];
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
void add(int u,int v,int cap)
{
    e[cnt].next=head[u];
    head[u]=cnt;
    e[cnt].to=v;
    e[cnt++].cap=cap;	
}
int bfs(int s,int t)
{
    while (!Q.empty()) Q.pop();
    for(int i=1;i<=(n<<1);i++) d[i]=0;
    d[s]=1,Q.push(s);
    while (!Q.empty()&&!d[t])
    {
        int x=Q.front();Q.pop();
        for(int i=head[x];~i;i=e[i].next)
        {
            int to=e[i].to;
            if (!d[to]&&e[i].flow<e[i].cap)
            {
                d[to]=d[x]+1;
                Q.push(to);
            }
        }	
    }
    return d[t];
}
int dfs(int x,int t,int flow)
{
    if (!flow||x==t) return flow;
    int ret=0,new_flow;
    for(int i=head[x];~i&&flow;i=e[i].next)
    {
        int to=e[i].to;
        if (d[x]+1==d[to])
        {
            new_flow=dfs(to,t,min(flow,e[i].cap-e[i].flow));
            e[i].flow+=new_flow;
            e[i^1].flow-=new_flow;
            ret+=new_flow;
            flow-=new_flow;		
        }		
    }
    return ret;	
}
int Dinic(int s,int t)
{
    int ret=0;
    while (bfs(s,t))
        ret+=dfs(s,t,INF);
    return ret;
}
int main()
{
    n=read(),m=read();
    st=read(),ed=read();
    for(int i=1;i<=n;i++) head[i]=-1;
    for(int i=1;i<=n;i++)
    {
        add(i,i+n,1);
        add(i+n,i,0);
    } 
    for(int i=1;i<=m;i++)
    {
        int u=read(),v=read();
        add(u+n,v,INF);
        add(v,u+n,0);
        add(v+n,u,INF);
        add(u,v+n,0);
    }
    printf("%d",Dinic(st+n,ed));
    return 0;	
}
```

