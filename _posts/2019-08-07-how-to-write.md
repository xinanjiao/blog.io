---
layout: post
title: 图解归并排序
date: 2019-8-9
categories: blog
tags: [算法,训练,归并排序]
description: 语言
---
原著链接：<https://www.cnblogs.com/chengxiao/p/6194356.html><br/>
### 基本思想
归并排序（MERGE-SORT）是利用归并的思想实现的排序方法，该算法采用经典的分治（divide-and-conquer）策略（分治法将问题分(divide)成一些小的问题然后递归求解，而治(conquer)的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之)。
**分而治之**
![分治](/img/mergesort1.png)

可以看到这种结构很像一棵完全二叉树，本文的归并排序我们采用递归去实现（也可采用迭代的方式去实现）。分阶段可以理解为就是递归拆分子序列的过程，递归深度为log2n。

### 合并相邻有序子序列
再来看看治阶段，我们需要将两个已经有序的子序列合并成一个有序序列，比如上图中的最后一次合并，要将[4,5,7,8]和[1,2,3,6]两个已经有序的子序列，合并为最终序列[1,2,3,4,5,6,7,8]，来看下实现步骤。
![合并](/img/mergesort2.png)
![合并](/img/mergesort3.png)

### 归并排序求逆序对
题目链接；<https://vjudge.net/contest/317879#problem/D><br/>
题目大意：给定一个1-N的排列A1, A2, ... AN，如果Ai和Aj满足i < j且Ai > Aj，我们就称(Ai, Aj)是一个逆序对。  <br>
求A1, A2 ... AN中所有逆序对的数目<br/>
大神题解:<https://blog.csdn.net/qq_41550842/article/details/81215935><br/>
题目思路：逆序对可以用到归并排序来求，实际上是一道模板题，逆序对的个数可以由数学公式推出，归并排序，先递归分开，然后各个排序，所以代码中有递归和排序，排序上图说的很清楚，由临时b数组保存，逐个比较，比较的过程实际上就可以得到逆序对个数，这时在稍加归纳就行了<br/>

code
  
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e5+1;
    const int INF=0x3f3f3f3f;
    int a[maxn],b[maxn];
    ll cnt=0;
    void merge_sort(int l,int r)
    {
    if(l==r)
        return ;
    int mid=(l+r)/2;
    merge_sort(l,mid);//递归分开
    merge_sort(mid+1,r);
    //merge_(l,r,mid);
    int cur=l,p=l,q=mid+1;
    while(p<=mid||q<=r)//排序
    {
        if(q>r||(a[p]<=a[q]&&p<=mid))
        {
            b[cur++]=a[p++];
        }
        else
        {
            b[cur++]=a[q++];
            cnt+=(ll)(mid-p+1);//数学公式推出
        }
    }
    fro(i,l,r+1)
    a[i]=b[i];
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    scanf("%d",&n);
    fro(i,1,n+1)
    {
        scanf("%d",&a[i]);
    }
    merge_sort(1,n);
    printf("%lld\n",cnt);
    }









