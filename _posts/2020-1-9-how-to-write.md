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











