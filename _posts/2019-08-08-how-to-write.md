---
layout: post
title: 动态规划之背包
date: 2019-8-8
categories: blog
tags: [算法,动态规划,训练]
description: 语言
---

## 01背包，完全背包，二维背包，多重背包
备用大神链接:<https://blog.csdn.net/yimingsilence/article/details/82628757><br/>
我就以做的题来作为例子。

## 01背包
前面有一个题，叫骨头收集者具体在训练第6天里面，那是一道01背包模板题，既然是模板题，那就有模板。<br/>
01背包问题大致问法:<br>
有N件物品和一个容量为V的背包。第i件物品的费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使价值总和最大<br/>
有时候问你最小，你把max改为min就行了<br>
二维dp，模板<br/>

    for(int i=0;i<a;i++)
    {
    	for(int j=0;j<=W;j++)
    	{
    		if(j<w[i])//装不下
    		dp[i+1][j]=dp[i][j];
    		else
    		dp[i+1][j]=max(dp[i][j],dp[i][j-w[i]]+v[i]);
    	}
    }

dp一维模板:<br/>

    for(int i=0;i<a;i++)
    {
    	for(int j=W;j>=w[i];j--)//W为总重量
    	{
    		dp[j]=max(dp[j],dp[j-w[i]]+v[i]);
    	}
    }


## 完全背包
问法:<br/>
有N种物品和一个容量为V的背包，每种物品都有无限件可用。第i种物品的费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使这些物品的费用总和不超过背包容量，且价值总和最大。<br/>
与01背包不同的是，它阔以无限取物品，所以来看看他的模板，与01背包类似<br/>

dp二维模板:

    for(int i=0;i<a;I++)
    {
    	for(int j=0;j<=W;j++)
    	{
    		if(j<w[i])
    		dp[i+1][j]=dp[i][j];
    		else
    		dp[i+1][j]=max(dp[i][j],dp[i+1][j-w[i]]+v[i]);//与01背包不同
    	}
    }

dp一维模板：

     for(int i=0;i<a;i++)
     {
       for(int j=w[i];j<=W;j++)//与01背包唯一不同的是循环顺序
       {
       	dp[j]=max(dp[j],dp[j-w[i]]+v[i]);
       }
     }


题目实战：<br/>

### Piggy-Bank 
题目链接：<https://vjudge.net/contest/315044#problem/C><br/>
题目大意:大概意思就是：有一个存钱罐，告知你其空时的重量和当前重量，并给定一些钱币的价值和相应的重量，问存钱罐中最少有多少现金？<br/>
思路：由于每个钱币都可以有任意多，所以该问题为完全背包问题，要求我们在状态转移时，在dp[j]和dp[j-w[i]]+v[i]]中选择一个较小的作为值，其次，要求钱币重量和空余的重量要恰好达到总重量。这里max就要用min了<br/>

code:

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    int dp[maxn];
    int v[maxn];
    int w[maxn];
    int main()
    {
    int t;
     cin>>t;
    while(t--)
    {
      int m,n;
      cin>>n>>m;
      int total=m-n;
      int a;
      cin>>a;
      mem(dp,INF);
      fro(i,0,a)
      {
          cin>>v[i]>>w[i];
      }
      dp[0]=0;//第一个必须为0，不然后面都为0
      fro(i,0,a)
      {
          for(int j=w[i];j<=total;j++)
          {
              dp[j]=min(dp[j],dp[j-w[i]]+v[i]);
          }
      }
      if(dp[total]!=INF)
      cout<<"The minimum amount of money in the piggy-bank is "<<dp[total]<<"."<<endl;
      else
        cout<<"This is impossible."<<endl;
    }
    }

### 完全背包升级版  FATE 