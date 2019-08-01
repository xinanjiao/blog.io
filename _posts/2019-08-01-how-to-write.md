---
layout: post
title: ACM训练第10天
date: 2019-8-01
categories: blog
tags: [算法,二分+贪心,训练]
description: 语言
---

### 小事项 greedy
今天做的很多贪心题，都差不多贪心与优先队列结合起来了,顺势复习了一波优先队列。<br/>
优先队列与队列不同的是，它可以按照优先级的不同，弹出元素，不是先进先出，所以访问元素也不是front(),而是top(),另外优先顺序需要用结构体来声明，如 <br/>

    struct cmp
    {
       bool operator()(const int a,const int b)const 
       {
       	return a>b;
       }
    };
    priority_queue<ll,vector<ll>,cmp> s;

需要注意的是，排序的cmp，如上代码，与sort()不同的是，如果sort按照这个排法，是由大到小，而优先队列是先弹出小的，这个要注意，而且重载的是括号，不是小于符号，需要在结构体中重载，set也是这样（亲试有效)。<br/>

贪心加二分，二分不难，但贪心策略要选好，所以慢慢总结叭！<br/>


##题目（代码后补）

### exam (Codeforce 732D)
题目链接：<https://vjudge.net/problem/CodeForces-732D><br/>
网上题解：<https://blog.csdn.net/haut_ykc/article/details/52845645><br/>
这道题做了半天，现在还处于死模拟的阶段，没有想出一个好的贪心策略，网上题解讲的挺好的，好好研究研究，这题很有意义，贪心思想很明显<br/>

### B - Fence Repair 
题目链接：<https://vjudge.net/contest/315591#problem/B><br/>
huffuman原理，哈夫曼树是构造最优二叉树的方法，在离散数学中有所提及，这道题就用了这个思想，每次选最小的两个，怎么实现，就用优先队列<br/>

### F - 湫湫系列故事——消灭兔子 
题目链接：<https://vjudge.net/contest/315591#problem/F><br/>
优先队列+贪心，排好兔子血量，箭的威力，从大到小，一个个遍历兔子，一旦有箭的威力大于兔子的血量，加入优先队列，每遍历一次，出一次队列（巧妙之处）<br/>

### Commando War
题目链接:<https://vjudge.net/contest/315591#problem/D><br/>
求最小花费的时间，不用去模拟每次时间的减少，直接贪心最大，一个个比较<br/>






