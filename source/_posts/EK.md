---
title: 'EK'
date: 2018-07-10 19:41:54
tags: [网络流,模板]
categories: [OI]
password:
top: 0
mathjax: false
---
~~动能算法~~
<!--more-->
```c++
#include<cstdio>
#include<queue>
using namespace std;
const int M=100050,N=10050,INF=1<<25;
int n,m,st,ed,head[N],cnt=0,pre[N],d[N];
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
void update(int x,int to,int flow)
{
    for(int i=x;i!=to;i=e[pre[i]^1].to)
    {
        e[pre[i]].flow+=flow;
        e[pre[i]^1].flow-=flow;	
    }	
}
int bfs(int s,int t)
{
    while (!Q.empty()) Q.pop();
    for(int i=1;i<=n;i++) d[i]=0;
    d[s]=INF,Q.push(s);
    while (!Q.empty()&&!d[t])
    {
        int x=Q.front();Q.pop();
        for(int i=head[x];~i;i=e[i].next)
        {
            int to=e[i].to;
            if (!d[to]&&e[i].flow<e[i].cap)
            {
                pre[to]=i;
                d[to]=min(d[x],e[i].cap-e[i].flow);
                Q.push(to);
            }
        }	
    }
    return d[t];
}
int EK(int s,int t)
{
    int ret=0,new_flow;
    while (new_flow=bfs(s,t))
    {
        ret+=new_flow;
        update(t,s,new_flow);
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
    printf("%d",EK(st,ed));	
    return 0;	
}
```

