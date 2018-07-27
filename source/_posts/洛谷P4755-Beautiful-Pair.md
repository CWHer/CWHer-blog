---
title: 洛谷P4755 Beautiful Pair
date: 2018-07-26 19:59:34
tags: [分治]
categories: [OI]
password:
top: 0
mathjax: true
---
对于一个$$\left ( L,R \right )$$的区间
若其中最大值位置为*k* 
所有可行解可以分为三类

- *i*，*j* 在*k* 的两边
- *i，k* 或者*k，j*
- $$\left ( L,k-1 \right ),\left ( k+1,R \right )$$区间内

对于最后一种递归计算即可

对于第二种只需统计*1* 的个数

对于第一种
若$$\left ( L,k-1 \right ),\left ( k+1,R \right )$$均已升序排序
枚举其中一个区间
则另一个区间中可以匹配的元素单调递减
因此可以在线性时间内完成统计
至于怎么对$$\left ( L,k-1 \right ),\left ( k+1,R \right )$$进行排序，归并即可

最优复杂度$$O\left ( NlogN \right )$$
~~然而只要一组升序的数据就能卡成$$O\left ( N^{2}\right)$$~~
<!--more-->

```c++
#include<cstdio>
#include<algorithm>
#define LL long long
using namespace std;
const int N=200050;
int n,pos[N][25],log[N];
LL ans=0,w[N],t[N];
inline int read()
{
    register int x=0,t=1;
    register char ch=getchar();
    while (ch!='-'&&(ch<'0'||ch>'9')) ch=getchar();
    if (ch=='-') t=-1,ch=getchar();
    while (ch>='0'&&ch<='9') x=x*10+ch-48,ch=getchar();
    return x*t;
}
int get(int x,int y,int l,int r)
{
    if (w[x]>w[y]) return x;
    if (w[y]>w[x]) return y;
    int mid=(l+r)>>1;
    return abs(mid-x)<abs(mid-y)?x:y;
}
int find(int l,int r)
{
    int k=log[r-l+1]-1;
    return get(pos[l][k],pos[r-(1<<k)+1][k],l,r);
}
void merge(int l,int k,int r)
{
    int cnt=0,i,j;
    for(i=l,j=k+1;i<k,j<=r;)
        t[++cnt]=w[i]<w[j]?w[i++]:w[j++];
    for(;i<k;) t[++cnt]=w[i++];
    for(;j<=r;) t[++cnt]=w[j++];
    t[++cnt]=w[k];
    for(int i=l;i<=r;i++) w[i]=t[i-l+1];
}
void calc(int l,int r)
{
    if (l>=r) return;
    int k=find(l,r);
    calc(l,k-1);
    calc(k+1,r);
    for(int i=l;i<k;i++) ans+=w[i]==1;
    for(int i=k+1;i<=r;i++) ans+=w[i]==1;
    for(int i=l,j=r;i<k;i++)
    {
        while (j>k&&w[i]*w[j]>w[k]) j--;
        ans+=j-k;
    }
    merge(l,k,r);
}
void init()
{
    for(int i=1;i<=n;i++)
        log[i]=log[i-1]+(1<<log[i-1]==i);
    for(int i=1;i<=n;i++) pos[i][0]=i;
    for(int j=1;j<=log[n];j++)
        for(int i=1;i+(1<<j)-1<=n;i++)
            pos[i][j]=get(pos[i][j-1],pos[i+(1<<(j-1))][j-1],i,i+(1<<j)-1);
}
int main()
{
    n=read();
    for(int i=1;i<=n;i++) w[i]=read();
    for(int i=1;i<=n;i++) ans+=w[i]==1;
    init();
    calc(1,n);
    printf("%lld\n",ans);
    return 0;
}
```

