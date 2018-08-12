---
title: 'HDU3625 Examining the Rooms '
date: 2018-08-12 18:12:07
tags: [组合数学]
categories: [OI]
password:
top: 0
mathjax: true
---
若某些门之间成环，则破开环中任意一扇门可以打开环中所有门
于是问题等价于*n* 扇门形成*1-k* 个环有几种方案，再除总方案数
总方案显然是$$n!$$
第一类斯特林数参考[组合数学入门](https://cwher.github.io/2018/08/12/组合数学入门)
题目还限制一号门不能单独成环，需减去一号单独成环的方案数
因此方案数为
$$
\sum _{i=1}^{k}S_{n}^{i}-S_{n-1}^{i-1}
$$
代码就不放了