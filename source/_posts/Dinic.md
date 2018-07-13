---
title: 'Dinic'
date: 2018-07-10 19:45:48
tags: [网络流,模板]
categories: [OI]
password:
top: 0
mathjax: false
---
比EK快很多的Dinic
<!--more-->
```c++
#include<cstdio>
#include<queue>
using namespace std;
const int M=120050,N=10050,INF=1<<25;
int n,m,st,ed,head[N],cnt=0,d[N],cur[N];
struct edge{int to,next,flow,cap;} e[M<<1];
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
    for(int i=1;i<=n;i++) d[i]=0;
    d[s]=1,Q.push(s);
    while (!Q.empty()&&!d[t])
    {
        int x=Q.front();Q.pop();
        for(int i=head[x];~i;i=e[i].next)
        {
            int to=e[i].to;
            if (e[i].flow<e[i].cap&&!d[to])
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
    for(int &i=cur[x];~i&&flow;i=e[i].next)
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
    {
    	for(int i=1;i<=n;i++) cur[i]=head[i];
        ret+=dfs(s,t,INF);
    }
    return ret;
}
int main()
{
    n=read(),m=read();
    st=read(),ed=read();
    for(int i=1;i<=n;i++) head[i]=-1;
    for(int i=1;i<=m;i++)
    {
        int u=read(),v=read();
        add(u,v,read());
        add(v,u,0);
    }
    printf("%d",Dinic(st,ed));
    return 0;	
}
```

