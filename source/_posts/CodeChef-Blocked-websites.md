---
title: CodeChef Blocked websites
date: 2018-08-14 13:49:45
tags: [Trie,贪心]
categories: [OI]
password:
top: 0
mathjax: true
---
**题目大意：**
现有一种过滤器，可以屏蔽以该过滤器为前缀的网站
给出一些需要被屏蔽和不能被屏蔽的网站
无法满足条件输出$-1$
在满足条件的情况下，最小化过滤器的串长之和
按字典序输出所有过滤器


以所有不能被屏蔽网站的名字建立一棵$Trie$
考虑插入每个需要被屏蔽网站的名字的过程
- 若经过的所有节点均是不能被屏蔽的，则无解
- 否则，将开头到第一个未在$Trie$中出现的字符作为一个过滤器，标记该过滤器并退出插入过程
- 若发现当前名字以一个过滤器为前缀，则不需要新增过滤器，可直接退出插入过程

该贪心的正确性~~显然~~
<!--more-->
```c++
#include<cstdio>
#include<string>
#include<vector>
#include<cstdlib>
#include<algorithm>
using namespace std;
const int N=2e5+50,rt=0;
int n,cnt=0,ch[N][26],col[N],idx=0;
char str[N];
string s[N];
vector<string> ans;
inline char get()
{
    register char ch=getchar();
    while (ch!='+'&&ch!='-') ch=getchar();
    return ch;
}
void insert(string s)
{
    int o=rt;
    for(int i=0;i<s.size();i++)
    {
        if (!ch[o][s[i]-'a'])
            ch[o][s[i]-'a']=++cnt;
        o=ch[o][s[i]-'a'];
        col[o]=1;
    }
}
void find(string s)
{
    int o=rt;
    string w="";
    for(int i=0;i<s.size();i++)
    {
        if (!ch[o][s[i]-'a'])
        {
            w.append(1,s[i]);
            ch[o][s[i]-'a']=++cnt;
            ans.push_back(w);
            return;
        }
        w.append(1,s[i]);
        o=ch[o][s[i]-'a']; 
        if (!col[o]) return;
    }
    puts("-1"),exit(0);
}
int main()
{
    scanf("%d",&n);
    for(int i=1;i<=n;i++)
    {
        char opt=get();
        scanf("%s",str);
        if (opt=='+') 
            insert(str);
        else s[++idx]=str;
    }
    for(int i=1;i<=idx;i++) find(s[i]);
    sort(ans.begin(),ans.end());
    printf("%d\n",ans.size());
    for(int i=0;i<ans.size();i++)
        printf("%s\n",ans[i].c_str());
    return 0;
}
```

