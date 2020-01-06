---
layout: post
title: uva11134+hello 2020
date: 2020-1-4
categories: blog
tags: [贪心]
description: 语言
---

### Fabled Rooks UVA - 11134 贪心
#### 题目大意
在n* n的棋盘上放置n个车，使得任意两个车互不攻击并且每个车在给出的矩形框内，给出矩形框的左上角坐标和右上角坐标。如果有解输出1到n个车的坐标，误解则输出"IMPOSSIBLE"（没有引号）。
#### 分析
车的横坐标和纵坐标没有必然的联系。所以可以将二维坐标分为两个一维坐标的问题。即在多个区间内选择不会重复的n个数字，如果能选出，则记录坐标，否则输出impossble。这里贪心方法尤为关键。我第一次写的时候，是将矩形的坐标由x坐标从小到大排序，在从左到右扫过去。这样做是有缺陷的。比如(1,3),(1,3)(2,2).这三个坐标已经按照x坐标大小排序，本来是有答案的，但按照从左到右扫就会错误。
<p style="color: red">正解：将横坐标从大到小排序，然后再将每个坐标从y到x扫一遍，这样做可知是正确的，保证了小的坐标不会被占有。</p>

    bool bookl[5010];
    bool bookr[5010];
    struct node
    {
    int l,r,id;
    node(){}
    };
    bool cmpr(node a,node b)
    {
    if(a.l==b.l)
        return a.r<b.r;
    return a.l<b.l;
    }
    int main()
    {
    ios::sync_with_stdio(0);
     int t;
     while(cin>>t)
     {
         if(t==0)
            break;
            mem(bookl,0);
            mem(bookr,0);
         node a[5010],b[5010];
         P res[5010];
         for(int i=0;i<t;i++)
         {
             cin>>a[i].l>>b[i].l>>a[i].r>>b[i].r;
             a[i].id=i;
             b[i].id=i;
         }
         stable_sort(a,a+t,cmpr);
         stable_sort(b,b+t,cmpr);
         bool ok3=0;
         for(int i=t-1;i>=0;i--)
         {
             node s=a[i];
             bool ok1=0,ok2=0;
             for(int j=s.r;j>=s.l;j--)
             {
                 if(!bookl[j])
                 {
                     bookl[j]=1;
                     res[s.id].first=j;
                     ok1=1;
                     break;
                 }
             }
             if(!ok1)
             {
                 ok3=1;
                 break;
             }
             s=b[i];
             for(int j=s.r;j>=s.l;j--)
             {
                 if(!bookr[j])
                 {
                     bookr[j]=1;
                     res[s.id].second=j;
                     ok2=1;
                     break;
                 }
             }
             if(!ok2)
             {
                 ok3=1;
                 break;
             }
         }
         if(ok3)
            cout<<"IMPOSSIBLE"<<endl;
         else
         {
             for(int i=0;i<t;i++)
                cout<<res[i].first<<" "<<res[i].second<<endl;
         }
     }
    return 0;
    }











