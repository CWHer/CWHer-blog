---
title: CodeForces-294E Shaass the Great
date: 2018-08-12 20:08:49
tags: [树形结构]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目大意：**
给出一棵*n* 个节点树
去掉其中一条边，再重新加入一条长度相同的边
问新树中任意两点之间距离的总和最小是多少 


原树去掉一条边之后变为两棵树，分别记为*A*，*B*，它们的节点个数分别为$A_{sz}$，$B_{sz}$
设去掉边长度为*d*，新连接的两点分别为*u*，*v*
记*A* 中所有点到*u* 距离之和为$S_{u}$，同理$S_{v}$
记*A* 中任意两点的距离之和为$C_{A}$，同理$C_{B}$

新树中任意两点的距离和分两部分考虑
- 在*A*，*B* 树内，即$C_{A}+C_{B}$
- 在两棵树之间

  例如$x-u-v-y$，其中*x* 为*A* 中的节点，*y* 为*B* 中的节点
  将这样的路径分三部分考虑

  - 首先是$u-v$，不难发现共经过$A_{sz}*B_{sz}$次
  - 然后是$x-u$，每个$x-u$都会经过$B_{sz}$次
  - 同理$v-y$

全部加起来就是
$$
C_{A}+C_{B}+d*A_{sz}*B_{sz}+S_{u}*B_{sz}+S_{v}*A_{sz}
$$
观察到$C_{A},C_{B},d,A_{sz},B_{sz}$与$u,v$无关
因此只需找$S_{u},S_{v}$最小的$u,v$

关于$S$的计算分为两个部分，以*A* 树为例

- 第一遍*dfs* 求出每个节点*x* 的子树大小和子树中节点到*x* 的距离和
$$
S_{x}+=S_{x_{son}}+val_{x-x_{son}}*sz_{x_{son}}
$$

- 第二遍*dfs* 
$$
S_{x_{son}}=S_{x}+val_{x-x_{son}}*\left (A_{sz}-sz_{x_{son}}  \right )-val_{x-x_{son}}*sz_{x_{son}}
$$
​	就是把中心点从*x* 移到$x_{son}$，$\left (A_{sz}-sz_{x_{son}}  \right )$多经过一条边，$sz_{x_{son}}$少经过一条边

有了$S$，$C$就好计算了
$$
C_{A}=\frac{\sum _{x \in A} S_{x}}{2}
$$
每条边$x-y$，会在*x*，*y* 各计算一次
<!--more-->
```c++
#include<cstdio>
#include<vector>
#define LL long long
using namespace std;
const int N=5050;
const LL INF=1LL<<60;
struct edge{int u,v,val;}t[N];
LL sz[N],c[N],ans=INF;//c[x]   所有点到x的sum
LL s1,s2,r1,r2,n,size;//sum{Any 2}   min{Any c[x]}
vector<int> e[N],g[N];
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
void dfs(int o,int fa)
{
    sz[o]=1,c[o]=0;
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (to!=fa)
        {
            dfs(to,o);
            sz[o]+=sz[to];
            c[o]+=c[to]+sz[to]*g[o][i];    
        }
    }
}
void calc(int o,int fa,LL &sum,LL &x)
{
    sum+=c[o],x=min(x,c[o]);
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (to!=fa)
        {
            c[to]=c[o]+(size-sz[to]*2)*g[o][i];
            calc(to,o,sum,x);
        }
    }
}
int main()
{
    n=read();
    for(int i=1;i<n;i++) 
    {
        int u=read(),v=read();
        int val=read();
        t[i]=(edge){u,v,val};
        add(u,v,val);
        add(v,u,val);
    }
    for(int i=1;i<n;i++)
    {
        r1=r2=INF,s1=s2=0;
        int u=t[i].u,v=t[i].v;
        dfs(u,v),size=sz[u];
        calc(u,v,s1,r1),s1/=2;
        dfs(v,u),size=sz[v];
        calc(v,u,s2,r2),s2/=2;
        ans=min(ans,sz[u]*sz[v]*t[i].val+s1+s2+r1*sz[v]+r2*sz[u]);
    }
    printf("%lld\n",ans);
    return 0;
}
```

