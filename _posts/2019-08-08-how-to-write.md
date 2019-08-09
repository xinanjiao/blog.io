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
题目大意:<br/>
最近xhd正在玩一款叫做FATE的游戏，为了得到极品装备，xhd在不停的杀怪做任务。久而久之xhd开始对杀怪产生的厌恶感，但又不得不通过杀怪来升完这最后一级。现在的问题是，xhd升掉最后一级还需n的经验值，xhd还留有m的忍耐度，每杀一个怪xhd会得到相应的经验，并减掉相应的忍耐度。当忍耐度降到0或者0以下时，xhd就不会玩这游戏。xhd还说了他最多只杀s只怪。请问他能升掉这最后一级吗？<br/>
input<br/>
输入数据有多组，对于每组数据第一行输入n，m，k，s(0 < n,m,k,s < 100)四个正整数。分别表示还需的经验值，保留的忍耐度，怪的种数和最多的杀怪数。接下来输入k行数据。每行数据输入两个正整数a，b(0 < a,b < 20)；分别表示杀掉一只这种怪xhd会得到的经验值和会减掉的忍耐度。(每种怪都有无数个)<br/>
output:<br/>
输出升完这级还能保留的最大忍耐度，如果无法升完这级输出-1<br/>

思路：第一眼是完全背包，但是有忍耐度，和杀怪次数的限制，第一次做，我以经验值为背包，看满足经验值这个背包的最小忍耐度，我觉得是可行的，但是那个杀敌次数不好统计，看了下网上题解，引出概念二维背包，以忍耐度为背包，得出最大经验值，套用上述一维模板，因为多了杀敌次数限制，所以又多了一维，所以此题用多维背包解，也可套用二维模板，那么再加一维就是三维背包了，以此类推....<br/>

多维背包模板加详解:<https://blog.csdn.net/sinat_26019265/article/details/51474371><br/>

code 

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=1e2+10;
    const int INF=0x3f3f3f3f;
    int dp[maxn][maxn];
    int v[maxn];
    int w[maxn];
    int main()
    {
    int m,n,k,s;
    while(cin>>m>>n>>k>>s)
    {//忍耐值为背包
        int sum=0;
        mem(dp,0);
        mem(v,0);
        mem(w,0);
        dp[0][0]=0;
        fro(i,0,k)
        {
            cin>>w[i]>>v[i];
        }
        int ans=10000;
        fro(i,0,k)
        {
            for(int j=v[i];j<=n+1;j++)
            {
                fro(u,0,s)//次数限制 多一维
                {
                dp[j][u]=max(dp[j][u],dp[j-v[i]][u-1]+w[i]);
                if(dp[j][u]>=m)
                {
                    ans=min(ans,j);//寻找最小的经验值
                }
                }
            }
        }
        if(ans>n)
            cout<<-1<<endl;
        else
            cout<<n-ans<<endl;
    }
    }

## 多重背包
问法：<br/>
有N种物品和一个容量为V的背包。第i种物品最多有n[i]件可用，每件费用是c[i]，价值是w[i]。求解将哪些物品装入背包可使这些物品的费用总和不超过背包容量，且价值总和最大。<br/>
这种问题多出现与多重部分和问题中，问题大意是，n个数有不同的个数限制，问有多少种情况或者能不能组成K这个数<br/>

dp一维模板：

    for(int i=0;i<n;i++)
    {
    	for(int j=0;j<=k;j++)
    	{
    		if(dp[j]>=0)
    		dp[j]=m[i];
    		else if(j<a[i]||dp[j-a[i]]<=0)
            dp[j]=-1;
            else
            dp[j]=dp[j-a[i]]-1; 
    	}
    }

题目实战:<br/>

### Coins 
题目链接:<https://vjudge.net/contest/315044#problem/E><br/>
问题描述和多重组合数问题差不多，所以我不在重复，这里就模板改一一部分代码即可<br/>

code

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e5+1;
    const int INF=0x3f3f3f3f;
    int dp[maxn];
    int a[maxn],b[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    while(cin>>m>>n)
    {
        if(!m&&!n)
            break;
        fro(i,0,m)
        {
            cin>>a[i];
        }
        fro(j,0,m)
        {
            cin>>b[j];
        }
        mem(dp,-1);
        dp[0]=0;
        fro(i,0,m)
        {
            fro(j,0,n+1)
            {
                if(dp[j]>=0)
                    dp[j]=b[i];
                else if(j<a[i]||dp[j-a[i]]<0)
                    dp[j]=-1;
                else
                    dp[j]=dp[j-a[i]]-1;
            }
        }
        int sum=0;
        fro(i,1,n+1)//统计个数
        if(dp[i]>=0)
            sum++;
        cout<<sum<<endl;
    }
    return 0;
    }