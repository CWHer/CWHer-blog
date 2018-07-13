---
title: 'ISAP'
date: 2018-07-10 19:48:15
tags: [网络流,模板]
categories: [OI]
password:
top: 0
mathjax: false
---
ISAP+GAP优化+当前弧优化
<!--more-->
```c++
#include<cstdio>
#include<queue>
using namespace std;
const int M=100050,N=10050,INF=1<<25;
int n,m,st,ed,head[N],cnt=0,d[N],num[N],pre[N],cur[N];
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
void bfs(int s)
{
    for(int i=1;i<=n;i++) d[i]=n;
    d[s]=0,Q.push(s);
    while (!Q.empty())
    {
        int x=Q.front();Q.pop();
        for(int i=head[x];~i;i=e[i].next)
        {
            int to=e[i].to;
            if (d[x]+1<d[to]&&e[i^1].cap)
            {
                d[to]=d[x]+1;
                Q.push(to);
            }
        }	
    }
}
int update(int x,int to)
{
    int ret=INF;
    for(int i=x;i!=to;i=e[pre[i]^1].to)
        ret=min(ret,e[pre[i]].cap-e[pre[i]].flow);
    for(int i=x;i!=to;i=e[pre[i]^1].to)
    {
        e[pre[i]].flow+=ret;
        e[pre[i]^1].flow-=ret;	
    }
    return ret;
}
int ISAP(int s,int t)
{
    bfs(t);
    for(int i=1;i<=n;i++) num[d[i]]++;
    for(int i=1;i<=n;i++) cur[i]=head[i];
    int ret=0,x=s;
    while (d[s]<=n)
    {
        if (x==t) ret+=update(t,s),x=s;		
        int flag=0;
        for(int i=cur[x];~i;i=e[i].next)
        {
            int to=e[i].to;
            if (d[x]==d[to]+1&&e[i].flow<e[i].cap)
            {
                flag=1,cur[x]=i;
                pre[x=to]=i;
                break;			
            }		
        }
        if (!flag)
        {
            int idx=n-1;
            for(int i=head[x];~i;i=e[i].next)
                if (e[i].flow<e[i].cap)
                    idx=min(idx,d[e[i].to]);
            if (--num[d[x]]==0) break;
            num[d[x]=idx+1]++;
            cur[x]=head[x];
            if (x!=s) x=e[pre[x]^1].to;
        }
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
    printf("%d",ISAP(st,ed));
    return 0;	
}
```

