---
layout: post
title: ACM训练第19天(马拉车算法）
date: 2019-8-17
categories: blog
tags: [算法,马拉车算法,训练,kmp]
description: 语言
---

### 写在前面
字符串博大精深，训练也接近尾声，今天主要是补题，搞算法。嗯，就酱紫....加油

### 今日份get---Manacher算法（马拉车算法）
起初听到这个名字我还纳闷，是不是像尺取法一样通过算法运行像马拉车一样来命名这个名字，但是了解了以后完全不沾边，看了这个发明人名字后恍然大悟，ma na che 哈哈哈，中华文化博大精深啊，哈哈哈。学了一下午还是不太懂，有点抽象，后面还得深入研究研究。<br/>
马拉车我认为讲的很好的两个链接：<br/>
<https://blog.csdn.net/HappyRocking/article/details/82622881><br/>
<https://blog.csdn.net/the_star_is_at/article/details/53354958><br/>


## 题目
kmp next 数组 ，与马拉车

### C - Simpsons’ Hidden Talents next数组求最大前后缀
题目链接：<https://vjudge.net/contest/320131#problem/C><br/>
解题核心是把两个数组合并为一个，再用kmpnext数组的特殊性质求解<br/>


### D - 最长回文 马拉车算法模板
题目链接:<https://vjudge.net/contest/320131#problem/D><br/>
马拉车算法旨在解决一个字符串中最大回文串问题，与其他算法不同的是，他的复杂度仅为o(n)，线性复杂度，经典中的经典<br/>

### B - Period 循环节 next 数组
题目链接：<https://vjudge.net/contest/313340#problem/B><br/>
next数组之再升级，详情请了解下面这篇关于循环节的博客<br/>
循环节：<https://www.cnblogs.com/chenxiwenruo/p/3546457.html><br/>










