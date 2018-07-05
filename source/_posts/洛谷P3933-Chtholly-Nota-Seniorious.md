---
title: '洛谷P3933 Chtholly Nota Seniorious'
date: 2018-07-05 20:13:49
tags: [二分答案]
categories: [OI]
password:
top: 0
mathjax: false
---
神奇的二分答案题
<!--more-->
根据题目奇奇怪怪的限制，不难发现选择的行和列要连续
为了方便计算，将矩阵转置四次，每次只需考虑行要连续
二分一下ans进行检验
对于矩阵中的mins和maxs，显然二者不能分在一起
根据mins，maxs和二分出来的ans，贪心地确定每个值所在的分组，若不连续则不合法
```c++
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int N=2050,INF=1<<30;
int n,m,now,w[2][N][N],ans,mins=INF,maxs=-INF;
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void rotate()
{
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
            w[now^1][j][n-i+1]=w[now][i][j];
    swap(n,m),now^=1;
}
bool check(int k)
{
    int num=0;
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
            if (maxs-w[now][i][j]>k)
                num=max(num,j);
        for(int j=1;j<=num;j++)
            if (w[now][i][j]-mins>k) return 0;
    }	
    return 1;
}
void solve()
{
    int L=0,R=ans;
    while (L<R)
    {
        int mid=(L+R)>>1;
        if (check(mid))
            R=mid;
        else
            L=mid+1;
    }
    ans=R;
}
int main()
{
    n=read(),m=read(),now=0;
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++)
        {
            w[0][i][j]=read();
            maxs=max(maxs,w[0][i][j]);
            mins=min(mins,w[0][i][j]);
        }
    ans=maxs-mins;
    rotate(),solve();
    rotate(),solve();
    rotate(),solve();
    rotate(),solve();
    printf("%d",ans);
    return 0;
}
```

