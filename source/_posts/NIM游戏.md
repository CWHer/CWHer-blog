---
title: 'Nim游戏'
date: 2018-07-06 07:11:38
tags: [博弈]
categories: [OI]
password:
top: 0
mathjax: true
---
当且仅当$$\left (  a_{1} \right ) xor\left (  a_{2}\right ) ...\left ( a_{n} \right )=0$$为一个P
1. 最终状态为0
2. 对于一个$$\left (  a_{1} \right ) xor\left (  a_{2}\right ) ...\left ( a_{n} \right )=k\left ( k>0 \right )$$ 的状态，必存在一个$$a_{i}>k$$，使得下一步$$\left (  a_{1} \right ) xor\left (  a_{2}\right ) ...\left ( a_{n} \right )=0$$
3. 对于一个$$\left (  a_{1} \right ) xor\left (  a_{2}\right ) ...\left ( a_{n} \right )=0$$，不存在一种方案使得下一步$$\left (  a_{1} \right ) xor\left (  a_{2}\right ) ...\left ( a_{n} \right )=k\left ( k>0 \right )$$

