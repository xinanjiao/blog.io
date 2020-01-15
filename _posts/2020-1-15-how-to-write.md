---
layout: post
title: uva1149（贪心+中途相遇法思想）12145(思维)
date: 2020-1-15
categories: blog
tags: [贪心,中途相遇法]
description: 语言。
---
贪心的题，真的榨干了我仅有的智商......太难顶了:(

### Bin Packing UVA - 1149 贪心+中途相遇法

#### 题目大意
给定N（n<=10e5）个物品的重量Li,背包的容量为M，同时要求每个背包最多装两个物品，求至少要多少个背包。

#### 思路
思路一：<br>
从大到小排序，从第一个开始，除掉这个数找后面n-i个数中第一个小于等于M-L的那个数，将那个数标记。复杂度nlogn。苦于对upper_bound和lower_bound的捉摸不透，放弃了。<br>
思路二：<br>
收尾开始匹配，像中途相遇法一样一个个看和是否大于M。具体看代码：

    int a[maxn];
    void init(int n){
    fro(i,0,n)a[i]=0;
    }
    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
       
        int n,maxl;
        cin>>n;
        init(n);
        cin>>maxl;
        fro(i,0,n)cin>>a[i];
        int sum=0;
        sort(a,a+n,greater<int>());
        int l=0,r=n-1;
        while(l<=r){
            if(a[l]+a[r]<=maxl){
                l++;
                r--;
            }
            else
                l++;
               sum++;
        }
        cout<<sum<<endl;
    if(t)cout<<endl;
    }
    //out.close();
    return 0;
    }









