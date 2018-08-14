---
title: 'ZOJ3649 Social Net'
date: 2018-08-13 08:03:10
tags: [树形结构,倍增]
categories: [OI]
password:
top: 0
mathjax: true
---
乍一看以为裸的树剖
后来发现事情没那么简单，$c_{k}-c_{j}$是有顺序的，$j\leq k$


第一问很简单
第二问维护一大堆倍增数组

首先$fa$不用说，然后是$mx$，$mn$记录往上走的最大（最小）值
还需要$f$记录上面节点-下面节点的最大值，$g$记录下面节点-上面节点的最大值

预处理
$$
mx\left [ x \right ]\left [ i \right ]=max\left ( mx\left [ x \right ]\left [ i-1 \right ] ,mx\left [fa\left [  x\right ] \left [ i-1 \right ]\right ]\left [ i-1 \right ]\right )
$$
$$
mn\left [ x \right ]\left [ i \right ]=min\left ( mn\left [ x \right ]\left [ i-1 \right ] ,mn\left [fa\left [  x\right ] \left [ i-1 \right ]\right ]\left [ i-1 \right ]\right )
$$
$$
f\left [ x \right ]\left [ i \right ]=max\left ( f\left [ x \right ]\left [ i-1 \right ] ,f\left [fa\left [  x\right ] \left [ i-1 \right ]\right ]\left [ i-1 \right ],mx\left [ fa\left [  x\right ] \left [ i-1 \right ] \right ]\left [ i-1 \right ]-mn\left [ x \right ]\left [ i-1 \right ]\right )
$$
$$
g\left [ x \right ]\left [ i \right ]=max\left ( g\left [ x \right ]\left [ i-1 \right ] ,g\left [fa\left [  x\right ] \left [ i-1 \right ]\right ]\left [ i-1 \right ],mx\left [ x \right ]\left [ i-1 \right ]-mn\left [ fa\left [  x\right ] \left [ i-1 \right ] \right ]\left [ i-1 \right ]\right )
$$

有了这些倍增数组之后就好求解了
记$LCA\left ( u,v \right )$为$pre$，将$\left ( u,v \right )$拆为$\left ( u,pre \right )$，$\left ( pre,v \right )$两条链
首先$max\left ( pre,v \right )-min\left (  u,pre\right )$肯定合法

然后考虑怎么求$\left ( u,pre \right )$的答案
设在倍增时已跳到$x$，用一个$num$记录$$\left ( u,x \right )$$的最小值
则下一次跳跃时
$$
ans=max\left \{ f\left [ x \right ] \left [ i \right ],mx\left [ x \right ]\left [ i \right ]-num\right \}
$$
之后更新$num$
$$
num=min\left \{ mn\left [ x \right ]\left [ i \right ] \right \}
$$
同理可求$\left ( pre,v \right )$的答案，用$num$记录最大值即可
<!--more-->
```c++
#include<cstdio>
#include<cstring>
#include<vector>
#include<algorithm>
using namespace std;
const int N=50050,rt=1,INF=1<<30;
struct edge{int u,v,val;}t[N];
int n,m,w[N],f[N],sum;
int fa[N][25],dep[N],log[N];
int mx[N][25],mn[N][25],dp[2][N][25];   //max{pre-son}   max{son-pre}
vector<int> e[N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
bool cmp(edge a,edge b){return a.val>b.val;}
int find(int x){return f[x]==x?x:f[x]=find(f[x]);}
void unite(int u,int v){f[find(u)]=find(v);}
void dfs(int o)
{
    for(int i=1;i<log[dep[o]];i++)
    {
        fa[o][i]=fa[fa[o][i-1]][i-1];
        mx[o][i]=max(mx[o][i-1],mx[fa[o][i-1]][i-1]);
        mn[o][i]=min(mn[o][i-1],mn[fa[o][i-1]][i-1]);
        dp[0][o][i]=max(dp[0][o][i-1],mx[fa[o][i-1]][i-1]-mn[o][i-1]);
        dp[0][o][i]=max(dp[0][o][i],dp[0][fa[o][i-1]][i-1]);
        dp[1][o][i]=max(dp[1][o][i-1],mx[o][i-1]-mn[fa[o][i-1]][i-1]);
        dp[1][o][i]=max(dp[1][o][i],dp[1][fa[o][i-1]][i-1]);
    }
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (!dep[to])
        {
            dep[to]=dep[o]+1;
            fa[to][0]=o;
            mx[to][0]=max(w[o],w[to]);
            mn[to][0]=min(w[o],w[to]);
            dp[0][to][0]=w[o]-w[to];
            dp[1][to][0]=w[to]-w[o];
            dfs(to);
        }
    }
}
int LCA(int u,int v)
{
    if (dep[u]<dep[v]) swap(u,v);
    for(int i=0;i<=log[dep[u]-dep[v]];i++)
        if (((dep[u]-dep[v])>>i)&1) u=fa[u][i];
    if (u==v) return u;
    for(int i=log[dep[u]];i>=0;i--)
        if (fa[u][i]!=fa[v][i])
            u=fa[u][i],v=fa[v][i];
    return fa[u][0];
}
int MAX(int u,int v)
{
    int ret=0;
    if (dep[u]<dep[v]) swap(u,v);
    for(int i=0;i<=log[dep[u]-dep[v]];i++)
        if (((dep[u]-dep[v])>>i)&1)
            ret=max(ret,mx[u][i]),u=fa[u][i];
    return ret;
}
int MIN(int u,int v)
{
    int ret=INF;
    if (dep[u]<dep[v]) swap(u,v);
    for(int i=0;i<=log[dep[u]-dep[v]];i++)
        if (((dep[u]-dep[v])>>i)&1)
            ret=min(ret,mn[u][i]),u=fa[u][i];
    return ret;
}
int pre(int u,int v)
{
    int ret=0,num=INF;
    for(int i=log[dep[u]];i>=0;i--)
        if (((dep[u]-dep[v])>>i)&1) 
        {
            ret=max(ret,dp[0][u][i]);
            ret=max(ret,mx[u][i]-num);
            num=min(num,mn[u][i]);
            u=fa[u][i];
        } 
    return ret;
}
int nxt(int u,int v)
{
    int ret=0,num=0;
    for(int i=log[dep[u]];i>=0;i--)
        if (((dep[u]-dep[v])>>i)&1) 
        {
            ret=max(ret,dp[1][u][i]);
            ret=max(ret,num-mn[u][i]);
            num=max(num,mx[u][i]);
            u=fa[u][i];
        }
    return ret;
}
int calc(int u,int v)
{
    int top=LCA(u,v);
    int ret=MAX(v,top)-MIN(u,top);
    ret=max(ret,pre(u,top));
    ret=max(ret,nxt(v,top));
    return ret;
}
int main()
{
    for(int i=1;i<N;i++)
        log[i]=log[i-1]+(1<<log[i-1]==i);
    while (~scanf("%d",&n))
    {
        for(int i=1;i<=n;i++) w[i]=read();
        for(int i=1;i<=n;i++) f[i]=i;
        for(int i=1;i<=n;i++) e[i].clear();
        m=read(),sum=0;
        for(int i=1;i<=m;i++)
        {
            int u=read(),v=read();
            t[i]=(edge){u,v,read()};
        }
        sort(t+1,t+m+1,cmp);
        for(int i=1;i<=m;i++)
        {
            int u=t[i].u,v=t[i].v;
            if (find(u)!=find(v))
            {
                sum+=t[i].val;
                e[u].push_back(v);
                e[v].push_back(u);
                unite(u,v);
            }
        }
        printf("%d\n",sum);
        memset(dep,0,sizeof(dep));
        memset(fa,0,sizeof(fa));
        memset(mx,0,sizeof(mx));
        memset(mn,63,sizeof(mn));
        memset(dp,0,sizeof(dp));
        dep[rt]=1,dfs(rt);
        int T=read();
        while (T--) 
        {
            int u=read(),v=read();
            printf("%d\n",calc(u,v));
        }
    }
    return 0;
}
```



