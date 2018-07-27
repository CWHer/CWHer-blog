---
title: Gauss消元
date: 2018-07-27 20:19:45
tags: [模板,Gauss]
categories: [OI]
password:
top: 0
mathjax: false
---
消元形成倒三角
<!--more-->
```c++
#include<cstdio>
#include<cmath>
#include<cstdlib>
#include<algorithm>
using namespace std;
const int N=105;
const double eps=1e-8;
int n;
double f[N][N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
void Gauss()
{
    for(int i=1;i<=n;i++)
    {
        int t=i;
        for(int j=i+1;j<=n;j++)
            if (fabs(f[j][i])>fabs(f[t][i])) t=j;
        for(int j=i;j<=n+1;j++) swap(f[i][j],f[t][j]);
        if (fabs(f[i][i])<eps) 
        {
            puts("No Solution");
            exit(0);
        }
        for(int j=i+1;j<=n;j++)
        {
            double t=f[j][i]/f[i][i];
            for(int k=i;k<=n+1;k++)
                f[j][k]-=f[i][k]*t;
        }
    }
    for(int i=n;i>0;i--)
    {
        for(int j=i+1;j<=n;j++)
            f[i][n+1]-=f[j][n+1]*f[i][j];
        f[i][n+1]/=f[i][i];
    }
}
int main()
{
    n=read();
    for(int i=1;i<=n;i++)
        for(int j=1;j<=n+1;j++) f[i][j]=read();
    Gauss();
    for(int i=1;i<=n;i++) 
        printf("%.2lf\n",f[i][n+1]);
    return 0;
}
```

