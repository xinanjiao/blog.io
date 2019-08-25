---
layout: post
title: ACM训练第13天
date: 2019-8-11
categories: blog
tags: [算法,数据结构,数学,训练]
description: 语言
---
## 题目（代码后补）

## 线段树

### I Hate It （线段树区间查询+区间修改）
题目链接：<https://vjudge.net/contest/318111#problem/B><br/>
符合区间加法，最大值=max（左区间最大值，右区间最大值）<br/>
题目大意：很多学校流行一种比较的习惯。老师们很喜欢询问，从某某到某某当中，分数最高的是多少。 
这让很多学生很反感。 

不管你喜不喜欢，现在需要你做的是，就是按照老师的要求，写一个程序，模拟老师的询问。当然，老师有时候需要更新某位同学的成绩。<br/>
<p style="color: red;">线段树用到的地方只要符合区间加法就行，什么叫符合区间加法，像这道题，所有的最大值，就是左边最大值与右边最大值的最大值，最小值如此，加法和也如此。</p>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int a[maxn];
     int tree[maxn*4];
    void bulidtree(int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a[l];
        return;
    }
    int mid=(r+l)/2;
    bulidtree(l,mid,rt*2);
    bulidtree(mid+1,r,rt*2+1);
    tree[rt]=max(tree[rt*2],tree[rt*2+1]);
    }
    void change(int pos ,int a,int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a;//max(a,tree[rt]);
        return;
    }
    int mid=(r+l)/2;
    if(pos<=mid)
        change(pos,a,l,mid,rt*2);
    else
        change(pos,a,mid+1,r,rt*2+1);
    tree[rt]=max(tree[rt*2],tree[rt*2+1]);
    }
    ll query(int al,int ar,int l,int r,int rt)
    {
    if(al<=l&&ar>=r)
    {
        return tree[rt];
    }
    int mid=(r+l)/2;
    ll maxl=-1,maxr=-1;
    if(al<=mid)
        maxl=max(query(al,ar,l,mid,rt*2),maxl);
    if(ar>mid)
        maxr=max(query(al,ar,mid+1,r,rt*2+1),maxr);
    return max(maxr,maxl);
    }
    int main()
    {
    int m,n;
    while(scanf("%d%d",&m,&n)!=EOF)
    {
        fro(i,1,m+1)
        {
            scanf("%d",&a[i]);
        }
        bulidtree(1,m,1);
       char c;
       int a,b;
       fro(i,0,n)
       {
           getchar();
           scanf("%c %d %d",&c,&a,&b);
           if(c=='U')
           {
               change(a,b,1,m,1);
           }
           else if(c=='Q')
           {
               printf("%lld\n",query(a,b,1,m,1));
           }
       }
    }
    }

### Improving the GPA (暴力枚举)
题目链接：<https://vjudge.net/contest/313310#problem/A><br/>
枚举出所有总和的可能<br/>
给出两个数m,n，代表在n个数的平均数是m，在表的信息下，叫你求出最大和最小的绩点。<br/>
我做的时候没有好的思路，结束后看了下网上的题解，由于是100的范围，可以暴力枚举即可。<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    int t;
    cin>>t;
    while(t--)
    {
        int score,n;
        cin>>score>>n;
        double maxs=-INF;
        double mins=INF;
        int sum=score*n;
        int i1,i2,j1,j2,k1,k2,l1,l2;
        fro(i,0,n+1)
        {
          i1=i*100;
          i2=i*85;
          fro(j,0,n-i+1)
          {
              j1=j*84;
              j2=j*80;
              fro(k,0,n-j-i+1)
              {
                  k1=k*79;
                  k2=k*75;
                  fro(l,0,n-j-i-k+1)
                  {
                      l1=l*74;
                      l2=l*70;
                     int maxss=i1+j1+k1+l1+(n-i-k-l-j)*69;
                     int minss=i2+j2+k2+l2+(n-i-k-l-j)*60;
                      if(maxss>=sum&&minss<=sum)
                      {
                          double ss=i*4.0+j*3.5+k*3.0+l*2.5+(n-i-k-l-j)*2.0;
                          double s2=i*4.0+j*3.5+k*3.0+l*2.5+(n-i-k-j-l)*2.0;
                          maxs=max(maxs,ss);
                          mins=min(mins,s2);
                      }
                  }
              }
          }
        }
        printf("%.4lf %.4lf\n",(double)mins/n,(double)maxs/n);
    }
    }

### Just a Joke (积分数学)HDU - 4969 
题目链接：<https://vjudge.net/problem/HDU-4969><br/>
积分物理公式推导（不会---）<br/>

### CS Course (线段树区间修改+区间取值) HDU - 6186
题目链接：<https://vjudge.net/contest/313323#problem/B><br/>
位运算拓展，注意tree数组用结构体要存三个数(尽量不用cin cout)<br/>

### Can you answer these queries? 线段树 区间相加
题目链接:<https://vjudge.net/contest/313335#problem/A><br/>
线段树模板，注意复杂度的降低，开根号最多开7次就会变为一<br/>

### 洛谷线段树 未完成 awsl 万恶的TLE
题目链接：<https://www.luogu.org/problem/P3372><br/>
一直TLE，注意懒标记的下放,待修改<br/>











