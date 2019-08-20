---
layout: post
title: ACM训练第22天（最短路径）
date: 2019-8-20
categories: blog
tags: [算法,训练,图论]
description: 语言
---

### 写在前面
进不了B组，我也不怪谁，自己水平问题，怪自己，努力提升自己相比怨天尤人来得快得多。继续加油！

### 今日get---图论最短路径之 Dijkstra算法，Bellmanford算法，Floyd算法
（待编辑）。。。。<br/>
注：以下博客以题目来说明算法，关于我自己对算法的理解，以后有时间补充。<br/>
三个算法总结：<https://blog.csdn.net/qq_35644234/article/details/60870719><br/>

## 题目

### 一题多解之 hud 2544 分别用三种算法求解 
【代码在vj中---提醒我自己】<br/>
题目链接：<https://vjudge.net/problem/HDU-2544><br/>
diskstra算法模板：<https://blog.csdn.net/u011721440/article/details/17164891><br/>

分别用以上三种算法均可解决，运行时间Floyd > Bellmanford > diskstra。三种算法均可解决正权图，其中Floyd和bellmandord算法可以解决负权图的最短路径，bellmanford可解决负权环问题。<br/>

### A - Alice's Print Service  c2 预处理+二分
题目链接：<https://vjudge.net/contest/321083#problem/A><br/>
一开始我没有想到后面可以比前面小，这道题主要是预处理，不是二分搜索将会超时！<br/>


### B - Wormholes 最短路之bellman-ford算法 负权+负权回路
题目链接：<https://vjudge.net/contest/320737#problem/B><br/>
bellmanford算法模板：<https://blog.csdn.net/a1097304791/article/details/89433549><br/>
判断负权环可以floyd算法也可以bellmandord算法，其中bellmanford有个升级版算法，也叫做队列优化bellmanford算法（SPFA),它使时间得以优化。后面好好看看，今天没看明白。<br/>








