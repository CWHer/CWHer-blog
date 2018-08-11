---
title: K短路
date: 2018-08-11 18:08:00
tags: [图论,A*,模板]
categories: [OI]
password:
top: 0
mathjax: true
---
其实$$K$$短路是有有理有据的做法的，~~但我不会啊~~

建立一个以$$d$$为关键字的优先队列
放入$$\left ( S,0 \right )$$，然后进行扩展
~~易证~~，当$$\left ( x,d \right )$$被第$$K$$次取出时，$$d$$为$$S$$到$$x$$的$$K$$短路

考虑用启发式优化来提高效率
到$$T$$的估计距离$$f_{x}$$可以为$$x$$到$$T$$的最短路
对于点$$x$$，$$g_{x}=d_{now}+f_{x}$$
优先队列以$$g$$为关键字

还可以继续优化
- 当取出其中某个点$$K$$次后，不需要将其再放入优先队列中
- 对于有距离要求的$$K$$短路，当取出元素的$$d$$大于给定值可直接退出

<!--more-->
```c++
#include<cstdio>
#include<vector>
#include<queue>
#include<cstring>
using namespace std;
const int N=5050,INF=1<<30;
struct edge{int to;double val;};
struct node{int id;double d;};
int n,m,ans=0,inq[N],cnt[N];
double E,f[N];
vector<edge> e[N],r[N];
queue<int> Q;
struct cmp
{
    bool operator()(node a,node b)
    {
        return a.d+f[a.id]>b.d+f[b.id];
    }
};
priority_queue<node,vector<node>,cmp> S;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while ((ch<'0'||ch>'9')&&ch!='-') ch=getchar();
    if (ch=='-') {t=-1;ch=getchar();}
    while (ch>='0'&&ch<='9') {x=x*10+ch-48;ch=getchar();}
    return x*t;
}
void SPFA(int s)
{
    for(int i=1;i<=n;i++) f[i]=INF;
    f[s]=0,inq[s]=1,Q.push(s);
    while (!Q.empty())
    {
        int x=Q.front();
        Q.pop(),inq[x]=0;
        for(int i=0;i<r[x].size();i++)
        {
            int to=r[x][i].to;
            double val=r[x][i].val;
            if (f[x]+val<f[to])
            {
                f[to]=f[x]+val;
                if (!inq[to]) inq[to]=1,Q.push(to);
            }
        }
    }
}
void A_star(int s,int t,int sz)
{
    S.push((node){s,0});
    while (!S.empty())
    {
        node x=S.top();
        if (cnt[x.id]>sz) return;
        S.pop(),cnt[x.id]++;
        if (x.d>E) return;
        if (x.id==t) 
        {
            ans++,E-=x.d;
            sz=E/x.d;
            memset(cnt,0,sizeof(cnt));
        }
        for(int i=0;i<e[x.id].size();i++)
        {
            int to=e[x.id][i].to;
            double val=e[x.id][i].val;
            if (x.d+val<=E) S.push((node){to,x.d+val});
        }
    }
}
int main()
{
    n=read(),m=read();
    scanf("%lf",&E);
    for(int i=1;i<=m;i++)
    {
        int u=read(),v=read();
        double val;
        scanf("%lf",&val);
        e[u].push_back((edge){v,val});
        r[v].push_back((edge){u,val});
    }
    SPFA(n);
    A_star(1,n,E/f[1]); 
    printf("%d\n",ans);
    return 0;
}
```

