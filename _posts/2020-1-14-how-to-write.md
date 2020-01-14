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


### Non-boring sequences UVA - 1608 （分治+中途相遇法）
#### 题目大意
如果一个序列的任意连续子序列中至少出现一次的元素，则称这个序列是不无聊的。输入一个n(n<=200000)个元素的序列A（各个元素均为10e9以内的非负整数），判断它是不是无聊的。
#### 思路
有个思路就是当你判断一个整数是出现一次的元素位置P时（下面叫做单元素），你就可以忽略它，因为要满足任意一个连续子序列至少存在一个单元素，那只要有这个单元素的子序列都满足，所以就分别求A[1...P-1],A[P+1....n]，看到这，我们就会想到**分治**，即用递归的方式分别以相同的方式求两边，如果都满足就输出none_boring ,反之输出 boring 。问题又回到怎样快速高效的确定区间该元素只出现一次？
<p style=" color: red">我们可以在o(n)的时间内求出每个元素离他最近的相同元素的位置，然后判断该位置是否在子序列的范围内，当然要分为左边离最近，和右边离最近，所以要预处理一下。然后将区间二分，分别遍历各个区间，看每个元素是否满足，所以判断函数如下：</p>

    bool check(int left,int right){
    //判断这个区间是否存在单个元素
    if(left>=right)
      return true;
    for(int i=left;i<=right;i++){
        if(l[i]<left&&r[i]>right)
            return check(left,i-1)&&check(i+1,right);
    }
    return false;
    }

<p style="color: red">很朴实的写法，最后TLE了。反观该算法，最坏情况就和快速排序一样都是n^2,当唯一的元素在最后一个位置时，这样的遍历最后还是遍历n遍。最后导致n^2</p>

#### 解决方法（中途相遇法）
<p style="color: bule">我们可以分别从左边和右边一起开始遍历，这样就可以大大减少遍历时间，最后复杂度恒为n*logn</p>

    bool check(int left,int right){
    //判断这个区间是否存在单个元素
    if(left>=right)
      return true;
      int i=left,j=right;
      while(i<=j){
        if(l[i]<left&&r[i]>right)
            return check(left,i-1)&&check(i+1,right);
         if(l[j]<left&&r[j]>right)
            return check(left,j-1)&&check(j+1,right);
        i++;
        j--;
      }
    return false;
    }

<p  style="color: red;font-size: 24px;">按理说，此题就该结束了，最后我还是一如既往的TLE了，我着实震惊。抓耳挠腮。最后居然是败给了memset()。我就好好谈一下memset。</p>

<p  style="color: red;font-size: 24px;">看了很多博客，也有人在oj中遇到用memset导致时间和空间大幅度增加的问题。总结一下：memset总滴来说内嵌还是循环初始化，只不过它通过很多编译器优化变得使普通循环快一点。但小数据最好还是循环，越大的数据memset的初始化时间效率越明显。这道题的T没说多大，我就在t--里面一直嵌套memset不断初始化，看见一个博客说，如果一直memset初始化，最后时间的增幅会越来越大。 </p>

关于memset性能的探讨：<https://blog.csdn.net/yunhua_lee/article/details/6381866>写的很好

    int num[maxn],l[maxn],r[maxn];
    map<int,int> mp;
    bool check(int left,int right){
    //判断这个区间是否存在单个元素
    if(left>=right)
      return true;
      int i=left,j=right;
      while(i<=j){
        if(l[i]<left&&r[i]>right)
            return check(left,i-1)&&check(i+1,right);
         if(l[j]<left&&r[j]>right)
            return check(left,j-1)&&check(j+1,right);
        i++;
        j--;
      }
    return false;
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        //mem(l,-1);
        //mem(r,INF);
        int n;
        cin>>n;
        fro(i,0,n) l[i]=-1;
        fro(i,0,n) r[i]=INF;
        mp.clear();
        fro(i,0,n){
            cin>>num[i];
        }
        //from left to right
        fro(i,0,n){
            if(!mp.count(num[i])){
                mp[num[i]]=i;
                //l[i]=-1;
            }
            else{
                l[i]=mp[num[i]];
                mp[num[i]]=i;
            }
        }
        //from right to left
        mp.clear();
        for(int i=n-1;i>=0;i--){
            if(!mp.count(num[i])){
                mp[num[i]]=i;
                //r[i]=INF;
            }
            else{
                r[i]=mp[num[i]];
                mp[num[i]]=i;
            }
        }
        if(check(0,n-1))//分治
            cout<<"non-boring"<<endl;
        else
            cout<<"boring"<<endl;
    }
    return 0;
    }