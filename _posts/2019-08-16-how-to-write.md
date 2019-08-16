---
layout: post
title: ACM训练第18天（kmp)
date: 2019-8-16
categories: blog
tags: [算法,训练,KMP]
description: 语言
---
### 写在前面
没有晋级b组，唉，有点失落，还有两场了，抓紧机会冲啊！！<br/>
今天的专题是字符串，说到字符串，字典树可以运用到字符串，今天温习了（重学了）字符串匹配算法KMP,复杂度o(m+n)的算法。<br/>
算法难吗？肯定难啊，难就不学了？不可能的，比起学算法，题目AC的那一刻，是最开心的，所以加油加油加油，冲冲冲。


### 今日份get---KMP
大佬博客（超详细）：<https://blog.csdn.net/v_july_v/article/details/7041827><br/>



## 题目（代码后补）
今天题不多，主要是把重要的几个题记录一下


### kmp优化 字符串匹配 B - Oulipo
题目链接：<https://vjudge.net/contest/320131#problem/B><br/>
一个模式串和一个主串，求主串中有几个模式串。一个思路，暴力枚举，一个个模拟，绝对超时。用到算法，kmp，高效解决字符串匹配问题<br/>

### A - Encoded Barcodes 字典树
题目链接：<https://vjudge.net/contest/313327#problem/A><br/>
一眼字典树，建树和查询都是模板，没什么好说的，主要是查询的字符串需要数字解码而来，我先一直没有看懂解码过程，解码就将数字对应为0或1，在将它看成二进制，再转为十进制。<br/>

### D - Zuhair and Strings  暴力枚举
题目链接：<https://vjudge.net/contest/313340#problem/D><br/>
看懂题意简单，前提是看懂（哭了），枚举26个字母，重头到尾扫一遍就行了<br/>

![哪吒](/img/lz5.jpg)




