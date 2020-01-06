---
layout: post
title: 扫描法+级角排序+codeforce 612div2
date: 2020-1-6
categories: blog
tags: [计算几何,扫描法,codeforce]
description: 语言
---

## 扫描法
<p style="color: red">类似于一种带有顺序的枚举法，例如：从左到右考虑数组的各个元素，也就是说从左到右扫描。它和普通枚举的重要区别是：扫描法往往枚举时维护一些重要的量。</p>

### Wine trading in Gergovia UVA - 11054 扫描法
### 题目大意
直线上有n个村庄，每个村庄要么买酒，要么卖酒，把k个单位的酒从一个村庄运到相邻村庄需要k个单位的劳动力，问最少需要多少劳动力，使得满足所有村庄的需求。给出n和n个村庄对酒的需求买酒（正数）卖酒（负数）。
### 理解
这道题非常的灵活。。乍一看好像很麻烦，需要搜索，但是实际上对酒的需求量和劳动力是一样的，可以利用这点，从左向右一次扫描每一个村庄，用last表示当前村庄的对酒的需求情况（到最后一个时，last必定为0）然后就相当于他要向下一个村庄转移这么多酒，即需要花费这么多的劳动力。

    int main()
    {
    ios::sync_with_stdio(0);
    int n;
    while(cin>>n)
    {
        if(n==0)
        break;
        ll a,res=0,last=0;
    for(int i=0;i<n;i++)
    {
        cin>>a;
        res+=abs(last);
        last+=a;
    }
    cout<<res<<endl;
    }
    return 0;
    }











