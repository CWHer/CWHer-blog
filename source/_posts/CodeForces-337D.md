---
title: CodeForces-337D
date: 2018-08-12 14:20:00
tags: [树形结构,DP]
categories: [OI]
password:
top: 0
mathjax: true
---
**问题大意：**
给出一棵*n* 个节点的树，每条边距离为*1*
其中有*m* 个节点受到了~~（来自东方的）~~神秘力量的影响
如果点*x* 有神秘力量来源，则距离*x* 小于等于*d* 的点均会被影响 
神秘力量来源只有一个，问有几个点可能有神秘力量来源



一个节点可能有神秘力量来源，当且仅当它与所有受影响点距离均小于等于*d*

首先考虑求子树内的最大距离
设$$f\left [ x \right ]$$为点*x* 到子树中节点的最远距离，不难得出
$$
f\left [ x \right ]=max\left \{ f\left [ x_{son} \right ]+1 \right \}
$$
要保证$$x_{son}$$中有被神秘力量影响的节点

设$$g\left [ x \right ]$$为*x* 到非子树中节点的最远距离
对于非子树中的节点，有两种可能
- 来自兄弟节点

$$
g\left [ x \right ]=max\left \{ f\left [ x_{brother} \right ]+2\right \}
$$
- 来自父节点

$$
g\left [ x \right ]=max\left \{ g\left [ x_{fa} \right ]+1\right \}
$$

对于来自兄弟节点的转移，暴力枚举会超时
考虑用$$son\left [ x \right ]$$记录*x* 子节点中距离最大值
仅在$$x=son\left [ x_{fa} \right ]$$遍历所有兄弟节点
<!--more-->
```c++
#include<cstdio>
#include<cstring>
#include<vector>
using namespace std;
const int N=100050;
int fa[N],son[N],f[N],g[N]; // subtree/not
int n,d,m,w[N],ans=0;
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
void calc(int o,int pre)
{
    if (w[o]) f[o]=0;
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (to!=pre)
        {
            fa[to]=o;
            calc(to,o);
            if (f[to]!=-1)
            {
                f[o]=max(f[to]+1,f[o]);
                if (!son[o]||f[to]>f[son[o]]) son[o]=to;
            }
        }
    }
}
void dfs(int o,int pre)
{
    if (w[o]) g[o]=0;
    if (o!=pre) 
    {
        if (g[pre]!=-1) g[o]=max(g[o],g[pre]+1);
        if (o!=son[pre]) g[o]=max(g[o],f[son[pre]]+2);
        for(int i=0;o==son[pre]&&i<e[pre].size();i++)
        {
            int to=e[pre][i];
            if (to!=o&&to!=fa[pre]&&f[to]!=-1) g[o]=max(g[o],f[to]+2);
        }
    }
    for(int i=0;i<e[o].size();i++)
    {
        int to=e[o][i];
        if (to!=pre) dfs(to,o);
    }
}
int main()
{
    n=read(),m=read(),d=read();
    for(int i=1;i<=m;i++) w[read()]=1;
    for(int i=1;i<n;i++)
    {
        int u=read(),v=read();
        e[u].push_back(v);
        e[v].push_back(u);
    }
    memset(f,-1,sizeof(f));
    memset(g,-1,sizeof(g));
    calc(1,1);
    dfs(1,1);
    for(int i=1;i<=n;i++)
        if (f[i]<=d&&g[i]<=d) ans++;
    printf("%d\n",ans);
    return 0;
}
```

