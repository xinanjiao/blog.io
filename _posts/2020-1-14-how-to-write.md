---
layout: post
title: codeforce div613 C +uva 1608 12174(关于memset性能的讨论)
date: 2020-1-14
categories: blog
tags: [数学,分治法,中途相遇法]
description: 语言
---

### Fadi and LCM CodeForces - 1285C （数学+思维）
#### 题目大意
输入一个数x(1<=x<=10e12),编程求出出a,b,满足lcm(a,b)=x，即a,b,的最小公倍数为x，当多种情况时满足max(a,b)最小。
#### 思路
自己想的时候没啥思路....比赛的时候这种题见得很多，多数都是放掉。晃眼一看有点像中国剩余定理，但并不是，这是逆着求最小公倍数的约数。说实话，我注意到那个最大值的最小值，我还想到了贪心+二分（狗头.jpg)，最后被我扼杀在摇篮。最后还是look大神的解法结束战斗。
<p style="color: red">解法还是很巧妙的。该题的式子可以抽象为a*b/gcd(a,b)=x，即求max(a,b)最小，就要贪心让gcd(a,b)=1,这样一来，有两个条件，即gcd(a,b)==1和x%a==0就可以满足条件。TQL%%%%。复杂度为sqrt(n)，可以满足。</p>

    int main(){
    ios::sync_with_stdio(0);
    ll n;
    cin>>n;
    ll a,b;
    ll c=sqrt(n);
    for(ll i=1;i<=c;i++){
        ll s=n/i;
        if(n%i==0&&__gcd(i,s)==1){
            a=i;
            b=s;
        }
    }
    cout<<a<<" "<<b<<endl;
    return 0;
    } 












