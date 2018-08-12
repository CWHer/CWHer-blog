---
title: HDU2643 Rank
date: 2018-08-12 15:26:37
tags: [组合数学]
categories: [OI]
password:
top: 0
mathjax: true
---
首先将排名相同的选手放入同一个集合
这样的集合个数可以为$$1-n$$
集合之间相对顺序并不清楚，因此是一个全排列
第二类斯特林数参考[组合数学入门](https://cwher.github.io/2018/08/12/组合数学入门)
设$$S\left ( n,k \right )$$为$$n$$个元素放入$$k$$个非空集合的方案数
不难得出总方案数为
$$
\sum_{i=1}^{n}S\left ( n,i \right )*i!
$$
代码就不放了