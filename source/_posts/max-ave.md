---
title: '最大化平均值'
date: 2018-07-05 20:00:55
tags: [二分答案]
categories: [OI]
password:
top: 0
mathjax: true
---
**问题描述：**
有n个物品的重量和价值分别是 wi 和 vi。从中选出k个物品使得单位重量的价值最大。

根据定义
$$
\sum_{i=1}^{k}v_{i}=x\sum_{i=1}^{k}w_{i}
$$
即
$$
\sum_{i=1}^{k}\left (v _{i}-xw_{i} \right )=0
$$
二分一下x，每次在$$\left (v _{i}-xw_{i} \right )$$贪心地找前k大的
复杂度$$O\left ( nlog^{2}n \right )$$
<!--more-->
```c++
#include<cstdio>
#include<algorithm>
using namespace std;
const int N=10050;
const double eps=1e-5;
int n,m,w[N],v[N];
double c[N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
bool check(double k)
{
	for(int i=1;i<=n;i++) c[i]=v[i]-k*w[i];
	sort(c+1,c+n+1);
	double ret=0;
	for(int i=n;i>n-m;i--) ret+=c[i];
	return ret>-eps;
}
int main()
{
	n=read(),m=read();
	double L=0,R;
	for(int i=1;i<=n;i++) w[i]=read();
	for(int i=1;i<=n;i++) 
		v[i]=read(),R=max((double)v[i],R);
	while (R-L>eps)
	{
		double mid=(L+R)/2;
		if (check(mid))
			L=mid;
		else
			R=mid;
	}
	printf("%.2lf",R);
	return 0;
}
```

