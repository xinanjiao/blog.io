---
layout: post
title: 二分交互
date: 2020-1-3
categories: blog
tags: [交互题]
description: 语言
---

### Glad to see you! CodeForces - 810D 二分+交互

#### 题目大意
交互题，在[1,n]中选出k个数，每次你可以给出一次询问a,b，回复为如果∣a−x∣≤∣b−y∣满足，那么回复"TAK"，负责回复"NIE"，其中x,y是K个数中分别与x,y距离最近的数。求60次询问内得到K个数中任意的两个。

#### 思路
1e5的大小，询问最多60次，想到二分。log1e5=16,询问3次，48，小于60，可行。多次查找的话想到二分之类的话，这里有个比较妙的技巧，每次给出(mid,mid+1)，然后判断在左边还是右边，得出1个后再用这个为边界找第2个。
<p style="color: red">害！二分是真的博大进深，昨天那个二分字符串就很强，今天两次二分更是秀，参考网上博客勉强理解，着实巧妙！</p>

    int lowbit(int x){return x&(-x);}
    int n,k;
    bool cheack(int l,int r)
    {
    if(r>n||l<0)
        return 1;//很重要，这个条件算是点睛之笔辽
    cout<<"1 "<<l<<" "<<r<<endl;
    fflush(stdout);
    string s;
    cin>>s;
    return s=="TAK";//为1表示必有一个数小于其中一个数
    }
    int binarysearch(int l,int r)
    {
    if(l>r)
        return 0;
        int ans=0,mid;
    while(l<=r)
    {
        mid=(l+r)/2;
        if(cheack(mid,mid+1))
            ans=mid,r=mid-1;
        else
            l=mid+1;
    }
    return ans;
    }
    int main()
    {
    ios::sync_with_stdio(0);
     cin>>n>>k;
     int x,y;
     x=binarysearch(1,n);
     y=binarysearch(1,x-1);//以x为边界
     if(!y)
        y=binarysearch(x+1,n);
        cout<<"2 "<<x<<" "<<y<<endl;
     fflush(stdout);
    return 0;
    }










