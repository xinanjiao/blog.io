---
layout: post
title: 搜索+线段树（变形）
date: 2019-9-23
categories: blog
tags: [算法,搜索,线段树]
description: 语言
---
### B - Wang Xifeng's Little Plot HDU - 5024（bfs）
题目链接：<https://vjudge.net/contest/328694#problem/B><br/>
题目大意：给出一张图，要求从一个点'.'开始，可以走八个方向，要求只拐一个弯，且等于90度，求能走的最长距离。<br/>
思路：<br/>
初始思路，遍历整张图，找到'.'，分别bfs两次，分别是从上下左右走，和左上左下右上右下，关于90度问题，通过数组的位置取余解决，果不其然T了，我就知道.....，补题，看了题解，题解bfs一次，用数组记录8个方向，且数组中的位置的下标可由取余得到相应的90度的路的步数，整体思路就是把每个点看为拐点，走8个方向。<br/>

### A - A Corrupt Mayor's Performance Art HDU - 5023 （线段树变形）
题目链接：<https://vjudge.net/contest/328694#problem/A><br/>
题目大意：给出n次操作加查询，P为改变数组的值，q为询问这个区间里有几种数并输出。<br/>











