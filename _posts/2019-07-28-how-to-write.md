---
layout: post
title: ACM训练第6天
date: 2019-7-28
categories: blog
tags: [算法,动态规划,训练]
description: 语言
---

### 动态规划（题目后补）
1. 背包问题 ：Bone Collector <br/>
题目链接:<https://vjudge.net/contest/315044#problem/B><br/>
典型01背包问题，dp的典型运用<br>

code
  
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
     #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
     #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=2e6+10;
    const int inf=0x3f3f3f3f;
    ll w[1001];
    ll v[1001];
    ll dp[1001][1001];
    int main()
    {
    int t;
    cin>>t;
    while(t--)
    {
        int m,sum;
        cin>>m>>sum;
        mem(w,0);
        mem(v,0);
        fro(i,0,m)
        cin>>v[i];
        fro(j,0,m)
        cin>>w[j];
        mem(dp,0);
        fro(i,0,m)
        {
            fro(j,0,sum+1)
            {
                if(j<w[i])//无法挑选
                 dp[i+1][j]=dp[i][j];//下一个
                 else
                dp[i+1][j]=max(dp[i][j],dp[i][j-w[i]]+v[i]);
            }
        }
        cout<<dp[m][sum]<<endl;
    }
    }
2. 数塔 <br/>
题目链接：<https://vjudge.net/contest/315044#problem/A><br/>
3. 最少拦截系统 :(最长上升子序列)<br/>
题目链接:<https://vjudge.net/contest/315044#problem/F><br/>
4. Ignatius and the Princess III（整数划分）<br/>
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=1028><br/>











