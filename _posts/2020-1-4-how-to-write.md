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

## codefroce Hello 2020

### New Year and Naming CodeForces - 1284A 模拟
#### 题目大意
题目有个图，看那个图就明白了，打表操作，唉，晚上脑壳不在线，改个bug改半天，服了


    int n,m;
    int main(){
    ios::sync_with_stdio(0);
    cin>>n>>m;
    string s1[40],s2[41];
    string s3[1010];
    fro(i,0,n)
    cin>>s1[i];
    fro(j,0,m)
    cin>>s2[j];
    int k=0,jj=m*n,i=0,j=0;
    while(jj--){
        s3[k++]=s1[i]+s2[j];
        i=(i+1)%n;
        j=(j+1)%m;
    }
    int kk;
    cin>>kk;
    while(kk--){
        int s;
        cin>>s;
        s--;
        cout<<s3[s%(m*n)]<<endl;
    }
    return 0;
    }

### New Year and Ascent Sequence CodeForces - 1284B 思维数学
#### 题目大意
给出n个数组，输入的第一个数维为数组的大小。当满足数组中有两个数满足ai < aj且(i < j)时，称数组为递增数组。问再给出的数组中，可以两两相加，可以加在后面，也可加在前面，也可同一个数组选两次，只要满足。问在给出的数组序列中，任选两个组成递增序列，问有多少种不同的选法。

#### 理解
比赛时，脑壳混了，最后一个样例始终想不出来怎么回事，可能提没读懂。<br>
我们发现，当一个序列为递增序列时，它和任意序列结合都为递增序列，这是第一种情况。情况数为2 * cnt-1,cnt为目前非递增序列的个数。<br/>
当这个序列不是递增序列时记录最大值和最小值，最后把所有不是递增序列的最大值放在一个数组中，最小值放在一个数组在中，对最小值进行排序，用二分查找第一个大于等于max数组值的位置，记录加上。<br>
复杂度：N* logN,n^2必超时

    int a[100100];
    int main()
    {
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    ll sum=0,sum2=n,cnt=0;
    vector<int> maxaa,minaa;
    while(n--)
    {
        mem(a,0);
        //mem(num,0);
        int len;
        cin>>len;
        bool ok=0;
        int maxa=-1,mina=INF;
        fro(i,0,len)
        {
            cin>>a[i];
            maxa=max(maxa,a[i]);
            mina=min(mina,a[i]);
        }
        for(int i=1;i<len;i++)
        {
            if(a[i]>a[i-1])
            {
                ok=1;
                break;
            }
        }
        //cout<<"??"<<endl;
        if(!ok)
        {
            maxaa.push_back(maxa);
            minaa.push_back(mina);
        }
        else
        {
            sum+=(sum2*2-1);
            sum2--;
            //cout<<sum<<endl;
        }
    }
    sort(minaa.begin(),minaa.end());
    int aa=minaa.size();
    for(int i=0;i<maxaa.size();i++)
    {
        sum+=(lower_bound(minaa.begin(),minaa.end(),maxaa[i])-minaa.begin());
    }
    cout<<sum<<endl;
    return 0;
    }







