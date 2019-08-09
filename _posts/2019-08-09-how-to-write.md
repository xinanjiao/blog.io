---
layout: post
title: 尺取法
date: 2019-8-07
categories: blog
tags: [算法,训练,尺取法]
description: 语言
---
参考博客:<https://blog.csdn.net/consciousman/article/details/52348439><br/>
## 尺取法
尺取法：顾名思义，像尺子一样取一段，借用挑战书上面的话说，尺取法通常是对数组保存一对下标，即所选取的区间的左右端点，然后根据实际情况不断地推进区间左右端点以得出答案。之所以需要掌握这个技巧，是因为尺取法比直接暴力枚举区间效率高很多，尤其是数据量大的时候，所以尺取法是一种高效的枚举区间的方法，一般用于求取有一定限制的区间个数或最短的区间等等。当然任何技巧都存在其不足的地方，有些情况下尺取法不可行，无法得出正确答案。<br/>

使用尺取法时应清楚以下四点：

1、  什么情况下能使用尺取法?  2、何时推进区间的端点？ 3、如何推进区间的端点？ 3、何时结束区间的枚举？

尺取法通常适用于选取区间有一定规律，或者说所选取的区间有一定的变化趋势的情况，通俗地说，在对所选取区间进行判断之后，我们可以明确如何进一步有方向地推进区间端点以求解满足条件的区间，如果已经判断了目前所选取的区间，但却无法确定所要求解的区间如何进一步得到根据其端点得到，那么尺取法便是不可行的。首先，明确题目所需要求解的量之后，区间左右端点一般从最整个数组的起点开始，之后判断区间是否符合条件在根据实际情况变化区间的端点求解答案。<br/>

<p style="color: red;font-size: 30px;" >多做题多总结题型</p>

### 经典例题

### poj 3061 Subsequence
题目链接：<https://vjudge.net/problem/POJ-3061><br/>
题目大意：给定一个序列，找出最短的子序列长度，使得其和大于或等于S。<br/>
分析：首先，序列都是正数，如果一个区间其和大于等于S了，那么不需要在向后推进右端点了，因为其和也肯定大于等于S但长度更长，所以，当区间和小于S时右端点向右移动，和大于等于S时，左端点向右移动以进一步找到最短的区间，如果右端点移动到区间末尾其和还不大于等于S结束区间的枚举。<br/>
这个题目区间和明显是有趋势的：单调变化，所以根据题目要求很容易求解，但是在使用之间需要对区间前缀和进行预处理计算。所谓预处理就是**前缀和**处理，前缀和可以缩短时间，复杂度o(n),查找复杂度o(1);

code
 
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int a[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    {
      int m,s;
      cin>>m>>s;
      fro(i,0,m)
      {
          cin>>a[i];
      }
      ll sum=0,l=0,r=0;
      ll ans=INF;
      while(l<m)
      {
          while(sum<s&&r<m)
          {
              sum+=a[r];
              r++;//像小虫虫一样伸展开
          }
          if(sum<s)
            break;
          ans=min(ans,r-l);
          sum-=a[l++];//缩回来
      }
      if(ans==INF)
        ans=0;
      cout<<ans<<endl;
     }
    }









