---
layout: post
title: 最大值最小化或最小值最大化问题（二分+贪心）
date: 2020-1-7
categories: blog
tags: [算法,二分+贪心]
description: 语言
---

在刷题时，总会遇到求最大值最小，最小值最大问题，也许它会暗喻是这样的一个问题。对于这样的一个问题，你会发现用dp和枚举都会超时超内存，或者说很麻烦，所以这是一个比较简单的解题方式。在暑假我也接触过类似的题目比如poj的疯牛之类的，但没有总结题型，今天再一次遇到，总结一波

### 二分逼近思想
<p style="color: red">对于难以直接确定解的问题,采取二分枚举+检验的思想. </p>

<p style="color: red">已知解为x,验证x是否满足要求.</p>

<p style="color: red">如果答案具有特定的范围，并且验证答案是否成立的函数具有单调性。则可以在范围内对答案进行二分验证，从而快速确定答案。</p>

### 对于答案的判断
<p style="color: red">在二分答案的时候需要判断，从而确定下一个范围。</p>

<p style="color: red">可以用一个bool Check(x)函数来判断，返回true表示满足,返回false表示不满足.</p>

<p style="color: red">可以类比数学函数f(x)>=0和f(x) < 0来理解.</p>

<p style="color: red">根据具体问题写出相应的Check函数往往就是解决问题的关键点。</p>


## 接下来就是类似题

### CSU 1976 忧伤的小明

#### 题目大意
作为老人的小明非常忧伤，因为他马上要被流放到本部去了，住进全左家垅最有历史感的11舍真是一件非常荣幸的事情。
搬行李是个体力活，小明发现自己的行李太多啦，所以他决定去买很多个袋子来装走。到了超市的小明发现，不同大小的袋子居然价格一样？？？虽然买最大的自然最赚，但是小明是名远近闻名的环保人士，他觉得袋子只要能装下他的行李就够了，并且为了不麻烦收银的小姐姐(⊙o⊙)…，他也只会购买同一种大小的袋子。因此他希望在能装下所有行李的前提下，袋子越小越好。同时为了避免弄乱行李，小明希望同一个袋子装的是位置连续相邻的行李。
小明摸了摸口袋发现自己带的钱最多能买N个袋子，数学特别差的他不知道到底该买多大的才合适，所以想靠你来解决这个问题了。
求的是满足的口袋的最小体积。

#### 理解
最后一句话可以看做**口袋最大值的最小值**，又回到最大值的最小问题。二分满足条件的体积，最后输出。

    ll bage[maxn];
    int n,m;
    bool check(ll a){
    ll sum=0,cnt=0;
    for(int i=0;i<m;i++){
        sum+=bage[i];
        if(sum>a){
            sum=bage[i];
            cnt++;
        }
    }
    if(cnt>=n)
        return true;
    else
        return false;
    }
    int main(){
    ios::sync_with_stdio(0);
     int t;
     cin>>t;
     while(t--){
        cin>>n>>m;
        ll sum=0,maxa=-1;;
        fro(i,0,m){
         cin>>bage[i];
         maxa=max(maxa,bage[i]);
         sum+=bage[i];
        }
        ll l=maxa,r=sum;
        while(l<r){
            ll mid=l+(r-l)/2;
            if(check(mid))
                l=mid+1;
            else
                r=mid-1;
                //cout<<mid<<endl;
        }
        cout<<l<<endl;
     }
    return 0;
    }

### Copying Books UVA - 714
#### 题目大意
一个抄写员要抄n本数，每本书有1<= x <= 10000000页，现在把这些数分配给k个抄书员，要求分配给某个抄写员的编号是连续的，而且每个抄写员的抄写速度是相同的，求所有书抄完所用的最少时间的分配方案。
#### 分析
咋一看很麻烦，枚举不知道怎么做，现在转化一下，即 一个序列，将序列划分为k个子序列，求出每个子序列的和，要使这些和中的最大值最小。又回到二分问题，二分枚举满足的和，判断根据能不能划分为K个子序列作为二分标准。输出有点麻烦。有个限制，题目说了，如果有多组解，要满足s(1)尽量小，如果还有多组解，即s(2)最小，以此类推。这个就运用贪心，从右往左贪心。

    ll a[maxn];
    int m,n;
    ofstream out;
    bool cheak(ll mid){
    ll cnt=0,sum=0;
    fro(i,0,m){
        sum+=a[i];
        if(sum>mid){
            sum=a[i];
            cnt++;
        }
    }
    if(cnt>=n)
        return true;
    else
        return false;
    }
    void output(ll l){
    int ans[2020];
    mem(ans,0);
    int k=n;
    ll sum=0;
    for(int i=m-1;i>=0;i--){
        sum+=a[i];
        if(sum>l){
            sum=0;
            ans[++i]=1;
            k--;
        }
    }
    int j=1;
    if(k>1){
        for(;j<m;j++){
            if(!ans[j]){
            ans[j]=1;
            k--;
            }
            if(k==1)
                break;
        }
    }
    cout<<a[0];
    fro(i,1,m){
         if(ans[i])
            cout<<" /";
         cout<<" "<<a[i];
    }
    cout<<endl;
    }
    void test(){
    fro(i,0,m){
    cout<<a[i]<<" ";
    }
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    //out.open("output.txt");
    while(t--){
        cin>>m>>n;
        ll maxa=-1;
        ll sum=0;
        mem(a,0);
        fro(i,0,m){
        cin>>a[i];
        maxa=max(maxa,a[i]);
        sum+=a[i];
        }
        //test();
        ll l=maxa,r=sum;
        while(l<r){
            ll mid=l+(r-l)/2;
            if(cheak(mid))
                l=mid+1;
            else
                r=mid;
        }
        //cout<<l<<endl;
        output(l);
    }
    //out.close();
    return 0;
    }







