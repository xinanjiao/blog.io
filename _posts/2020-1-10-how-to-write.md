---
layout: post
title: uva11093环形跑道（模拟）
date: 2020-1-10
categories: blog
tags: [模拟]
description: 语言
---

<p style="color: red">害！最近几天可能都做不了题了。前几天发小回来，数一数也有几年没见了叭，完了两天，昨天还去学校玩了一天，周末又要去江津，所以刷不了题了，慢慢滴确实开始忙了起来。”应酬”开始多了起来。慢慢滴题也开始变得很难，啃也不是一天就能完成的事情，所以要好好规划时间了。</p>

### Just Finish it up UVA - 11093 模拟+规律
#### 题目大意 
环形跑道上的加油站有n个，第i个加油站加油pi单位，开到下个加油站需要qi单位，求一个最小的起点编号使得从该点出发可以绕一圈回到该起点。
#### 分析
绕一圈，所花费的总油是相同的，所以先求出可不可以绕一圈，即加的有减去消耗的油是否小于0，小于0就不可能，反正遍历枚举。<br>
遍历数组，因为前提条件已经满足了，所以必有解。当存在加的油小于消耗的油时，记录下位置i+1+1,即当前位置的下一个位置，如果一直大于0下去，那就是那个值。


    struct node{
    int add,cost;
    node(){}
    };
    node a[maxn];
    int main(){
    ios::sync_with_stdio(0);
    int t,case1=1;
    cin>>t;
    while(t--){
        cout<<"Case "<<case1++<<": ";
        int n;
        cin>>n;
        ll sum=0,sum2=0;
        fro(i,0,n){
        cin>>a[i].add;
        sum+=a[i].add;
        }
        fro(i,0,n){
        cin>>a[i].cost;
        sum-=a[i].cost;
        }
        if(sum<0)
            cout<<"Not possible"<<endl;
        else{
            ll sum2=0,p;
            fro(i,0,n){
                sum2+=a[i].add;
                sum2-=a[i].cost;
                if(sum2<0){
                    sum2=0;
                    p=i+2;
                }
            }
            cout<<"Possible from station "<<p<<endl;
        }
    }
    return 0;
    }








