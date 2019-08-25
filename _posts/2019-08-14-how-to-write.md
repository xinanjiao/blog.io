---
layout: post
title: ACM训练第16天
date: 2019-8-14
categories: blog
tags: [算法,训练,数学]
description: 语言。
---

### 写在前面
今天个人赛，c1,c2,c3都打了，c1还是那样，神仙打架，第一题就卡我很久，主要是没想到那方面（太菜了）。c2 15年浙江省赛题，有简单的（我就好这口），不过第一题又是题意没搞清楚，一直wa,服了服了，英语是硬伤。。。其他中规中矩的规律模拟题（还是不太会），c3 数学+规律，有我做过的题，算运气好吧，这种题其中有规律，找出即可，有时候就是不好找。。。。（多练应该就行了嘿嘿..)。c3 要加强专题练习了

## 题目（code 后补）


### Break the Chocolate 规律+公式
题目链接:<https://vjudge.net/contest/313311#problem/A><br/>
题目给出巧克力的长宽高，叫你用分别手扳和用刀切成1x1x1的正方体分别要用最少的步骤,注意刀切可以重叠（当初一直没有注意到），刀切了之后，实际上你会发现，刀切都是2的多少次幂。<br/>

    const double EPS=1e-6;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    ll power(ll a)
    {
    ll cnt=1;
    fro(i,0,a)
    {
        cnt*=2;
    }
    return cnt;
    }
    ll change(int a)
    {
    ll pos=0;
    while(1)
    {
        if(power(pos)>=a)
            return pos;
        pos++;
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    int case1=1;
    while(t--)
    {
        int a[3];
        ll c,w,h;
        ll c1,w1,h1;
        ll sumn=0;
        ll sumh=0;
        cin>>a[0]>>a[1]>>a[2];
        sort(a,a+3);
        c=a[0];w=a[1];h=a[2];
        sumh=(c*w*h-1);
        if(c>1)
            sumn+=change(c);
        if(w>1)
             sumn+=change(w);
        if(h>1)
            sumn+=change(h);
        cout<<"Case #"<<case1++<<": "<<sumh<<" "<<sumn<<endl;
    }
    return 0;
    }

### Peak 水题
题目链接：<https://vjudge.net/contest/313326#problem/A><br/>
虽然是水题，但是题意没有读懂，以后长个记性，我以为那个数组下标必须是数组中的元素...不知道我咋想的<br/>

    const int maxn=5e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        int a[maxn];
        int n;
        cin>>n;
        fro(i,0,n)
        cin>>a[i];
        int pos;
        bool ok=false;
        bool ok1=false;
        fro(i,0,n)
        {
            if(a[i+1]>a[i])
                {
                    ok1=true;
                    continue;
                }
            else
               {
                   pos=i;
                   break;
               }
        }
        int i=0;
        //cout<<pos<<endl;
        for( i=pos+1;i<n;i++)
        {
            if(a[i-1]>a[i])
                continue;
            else
            {
                //ok=false;
                break;
            }
        }
        //cout<<i<<endl;
        if(pos!=0&&pos!=n-1&&i==n)
        puts("Yes");
       else
        puts("No");
    }
    return 0;
    }

###  CONTINUE...? ZOJ - 4033
题目链接：<https://vjudge.net/contest/313326#problem/J><br/>
规律题，构造。。。。（好好理解理解）<br/>

### UVA - 11526 H(n) 数学 整数分块
题目链接：<https://vjudge.net/contest/313337#problem/D><br/>
暴力超时，把除数分块，每次可以节约很多没意义的步骤<br/>


![哪吒](/img/lz4.jpg)








