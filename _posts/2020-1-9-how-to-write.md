---
layout: post
title: uva 11572+12627(尺取法，递归求解)
date: 2020-1-9
categories: blog
tags: [尺取法,递归]
description: 语言
---

### Unique Snowflakes UVA - 11572(滑动区间，尺取法)
#### 问题大意
输入长度为n(n<=1e6)的序列A，找到一个尽量长的连续子序列AL - AR，使得序列中没有相同的元素。
#### 分析
很经典的一道题，紫书上说是滑动区间，实际上就是尺取法，暑假接触过，这道题是用set实现的尺取法的移动，核心一样。但我还是佩服紫书上的方法二，我觉得很经典。

法一：

    int a[maxn];
    int main(){
    //ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--)
    {
        mem(a,0);
        int n;
        cin>>n;
        fro(i,0,n)
        {
            cin>>a[i];
        }
        set<int> s;
        int ans=-INF;
        int l=0,r=0;
        while(r<n)
        {
            while(r<n&&!s.count(a[r]))
            s.insert(a[r++]);
            ans=max(ans,r-l);
            s.erase(a[l++]);
        }
        cout<<ans<<endl;
    }
    }


法二：
用map求出last[i],即下标i的“上一个相同元素的下标”。例如：输入序列为3 2 4 1 3 2 3，当前区间是[1，3]即元素2，4，1，是否可以延申，下一个数是A[5]=3,它的上一个相同位置下标为0，不在区间中，可以延申。复杂度和上面相同，O(nlogn)

    int a[maxn],last[maxn];
    map<int,int> cur;
    int main(){
    int t,n;
    cin>>t;
    while(t--){
        cin>>n;
        cur.clear();
        fro(i,0,n){
            cin>>a[i];
            if(!cur.count(a[i]))
                last[i]=-1;
            else
                last[i]=cur[a[i]];
            cur[a[i]]=i;
        }
        int l=r=0,ans=0;
        while(r<n){
            while(r<n&&last[r]<l)r++;
            ans=max(ans,r-l);
            l++;
        }
        cout<<ans<<endl;
    }
    return 0;
    }

### Erratic Expansion UVA - 12627 (奇怪的气球膨胀 递归)
#### 题目大意
一开始有一个红气球。每小时后，一个红气球变成3个红气球和一个蓝气球，而一个蓝气球会变成4个蓝气球，如下图。分别是经过0，1，2，3小时后的情况，经过k小时后，第A-B行一共有多少个气球。例如,k=3,a=3,b=7，answer=14.
![uva](/img/uva12627.png)

#### 题目分析
看图的话，我的第一个意思是用一个数组存下每一行的红气球个数，但是，最大时间为30，即2^30次方，数组不可能开那么大。认真观察这个图，会发现，右下角全为蓝气球，可分为四个板块，最右下那个就不看了，其他三个都是k-1时间的图形组合的。实际上再细心观察，按紫书上的说法，可以有两种思考方法。我是第二种。
<p style="color: red">设g(k,i)为第k小时从i行开始往上数的所有红气球。所以答案为g(k,a)-g(k,b-1)。看图形。当输入的行数小于2^(k-1)时，第i行的红气球的和就为g(k-1,i)*2。当i大于2^(k-1)时，此时的红气球数为pow(3,k)(总红气球个数)-(pow(3,k-1)-g(2^(k-1)-i,k-1))个数，最后相减即可  </p>

    int k,a,b;
    ll solve(int k,int aa){
    if(aa==1&&k==0)
        return 1;
    else if(aa<=0)
        return 0;
    ll s=pow(2,k-1);
    if(aa<s)
    return 2*solve(k-1,aa);
    else
    return pow(3,k)-(pow(3,k-1)-solve(k-1,aa-s));
    }
    int main(){
    ios::sync_with_stdio(0);
    int t,case1=1;
    cin>>t;
    while(t--){
        cin>>k>>a>>b;
        cout<<"Case "<<case1++<<": ";
        cout<<solve(k,b)-solve(k,a-1)<<endl;
    }
    return 0;
    }









