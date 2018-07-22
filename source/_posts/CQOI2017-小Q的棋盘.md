---
title: '[CQOI2017]小Q的棋盘'
date: 2018-07-22 19:05:22
tags: [DP,树形结构,背包]
categories: [OI]
password:
top: 0
mathjax: true
---
*f* 记录第*i* 个点，访问*j* 条边，且返回*i* 点，经过的最大格点数量
*g* 则记录不返回的
$$
f\left[i\right]\left[j\right]=max\left\{ f\left[i\right]\left[k\right]+f\left[to\right]\left[j-k-2\right]\right\}\left ( to\in i_{son} \right )
$$
不在该子节点一去不回头
$$
g\left[i\right]\left[j\right]=max\left\{ g\left[i\right]\left[k\right]+f\left[to\right]\left[j-k-2\right]\right\}\left ( to\in i_{son} \right )
$$
在该子节点一去不回头
$$
g\left[i\right]\left[j\right]=max\left\{ f\left[i\right]\left[k\right]+g\left[to\right]\left[j-k-1\right]\right\}\left ( to\in i_{son} \right )
$$
<!--more-->
```c++
#include<cstdio>
#include<vector>
using namespace std;
const int N=200;
int n,m,f[N][N],g[N][N];	//f:ret g:no
vector<int> e[N];
void dfs(int o,int fa)
{
    f[o][0]=g[o][0]=1;
    for(int k=0;k<e[o].size();k++)
    {
        int to=e[o][k];
        if (to==fa) continue; 
        dfs(to,o);
        for(int i=m;i>0;i--)
            for(int j=0;j<i;j++)
            {
                if (i-j-2>=0)
                {
                    f[o][i]=max(f[o][i],f[to][j]+f[o][i-j-2]);
                    g[o][i]=max(g[o][i],f[to][j]+g[o][i-j-2]);			
                }	
                g[o][i]=max(g[o][i],g[to][j]+f[o][i-j-1]);			
            }
    }
    for(int i=1;i<=m;i++)
    {
        f[o][i]=max(f[o][i],f[o][i-1]),
        g[o][i]=max(g[o][i],g[o][i-1]);		
    }     
}
int main()
{
    scanf("%d%d",&n,&m);
    for(int i=1;i<n;i++)
    {
        int u,v;
        scanf("%d%d",&u,&v);
        e[u].push_back(v);
        e[v].push_back(u);
    }
    dfs(0,0);
    printf("%d\n",g[0][m]);	
    return 0;
}
```

