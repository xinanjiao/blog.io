---
layout: post
title: ACM训练第15天（字典树）
date: 2019-8-13
categories: blog
tags: [算法,训练,数据结构]
description: 文章金句。
---

### 昨天是TLE的一天，那么今天就是RE的一天，实际上也不是很多RE，只不过做到了一道，巨坑的题吗，light oj上的。RE解决策略：(1)数组访问是否越界。（2）数组是否开小了，有时候开大了也不行（3）输入输出尽量选用较快输入输出........

### 今日份get ---- 字典树
字典树入门精讲：<https://blog.csdn.net/jiutianhe/article/details/8076835><br/>
字典树入门题：<https://blog.csdn.net/qq_38891827/article/details/80532462><br/>

<p style="color: red;">字典树顾名思义就是把字符串像字典一样建成一棵树，树的结点没有字符，字符只是每个结点之间的连接线，字典树的查找和修改都是线性的，我喜欢用数组模拟字典树，root二维数组储存每个结点到下一个结点之间的字符，bool类型的一个数组判断下一个结点是否还有字符连接，可以用于查询操作，coun数组用于记录每个结点的字符的次数，由于查询和修改操作。</p>
## 题目（代码后补）

### 字典树 light oj 1224 DNA Prefix 坑得死人（数组开小了一直re）
题目链接：<https://vjudge.net/problem/LightOJ-1224><br/>
这就是坑人的题了，数组开大了re,开小了re，我也是服了，与也算是一道模板题，但需要注意的是，节约时间要读的时候确定最大值<br/>

### hdu 字典树 1251统计难题  模板题
题目链接：<https://vjudge.net/problem/HDU-1251><br/>
字典树题，结点记录前缀次数<br/>

### hdu 字典树 2072单词数  模板题
题目链接:<https://vjudge.net/problem/HDU-2072><br/>
字典树可以很好解决这个问题<br/>

### POJ 字典树 2001 Shortest Prefixes
题目链接:<https://vjudge.net/problem/POJ-2001><br/>
通过判断布尔标记ok解决。<br/>

### 01字典树 hdu 4825 Xor Sum 异或运算模板
题目链接：<https://vjudge.net/problem/HDU-4825><br/>
01字典树，注意异或等位运算法则，通过看异或运算顺便复习了一下快速幂。<br/>
大佬教你了解位运算：<https://www.luogu.org/blog/chengni5673/er-jin-zhi-yu-wei-yun-suan><br/>

### 字典树 POJ Phone List 3630
题目链接：<https://vjudge.net/problem/POJ-3630><br/>
re多次，最后发现是没有更新结点。。。。<br/>

![哪吒](/img/lz2.jpg)






