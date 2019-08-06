---
layout: post
title: ACM训练第7天
date: 2019-7-29
categories: blog
tags: [算法,动态规划,训练]
description: 语言
---

### 谈谈收获
 1. 今天教主叫他第一届ACM队员给我们做了一下一个小小的人生认识，果然是阿里云大佬，对事情的客观认识都很全面，主要从未来和立足于现在出发，分析了这个互联网时代公司的人才需求，考研的趋势愈发明显，又给我们讲了一下关于考研导师的选择，选择导师时要找对一个领域有足够深认识的导师，越著名的导师，可能越没有时间带你。还有就是这个时代的趋势，对一个有可能成为趋势的领域研究，不要研究没有可能的领域。他也谈谈了他对于ACM的认识。我也很认同他的看法，本科是一个打基础的阶段，编程和编程的正确性都将在这个阶段锻炼，所以他说，花多一点时间在这个基础的培养上，ACM是一个不错的选择，专心去做一件事，始终有收获的。上次教主也说，每个选择都有不同的价值，价值的判断就在于每个价值后的权重。

2. 今天也打了我的第一个组队赛，啊，好憋屈，上一次B组很简单的好吧，这一次不是一个级别了，可能还是太菜了，和队友足足憋了4个小时的B题，比赛完后看了看题解，几行+一个数学公式就行了，唉，继续加油吧，慢慢来，我也体会到，队友之间的沟通还是很重要的，配合也很重要。


### 动态规划（题目后补）
1. 最长上升子序列（LIS)<br>
题目：个人赛C2 array array array<br>
题目链接:<https://vjudge.net/contest/313304#problem/B><br/>
思路：分别求两次最长上升子序列，分别是正序和逆序，找到最小的序列数和数组长度减去移动次数比较，这里的最初上升子序列为优化过的方法，里面的lower_pound()为二分查找不大于一个数在数组中的位置，复杂度减少为n乘以logn<br/>

code：

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int a[maxn],dp[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        int n,k;
        cin>>n>>k;
        fro(i,0,n)
        cin>>a[i];
        int ans=0;
        int temp=0;
        //dp[0]=a[0];
        mem(dp,0);
        fro(i,0,n)
        {
            int p=upper_bound(dp,dp+temp,a[i])-dp;//二分法求dp[i]>=a[i]
            if(p==temp)
            {
                dp[temp++]=a[i];
                ans=max(ans,temp);
            }
            else
                dp[p]=a[i];
        }
        temp=0;
        mem(dp,0);
        pre(i,n-1,0)
        {
            int p=upper_bound(dp,dp+temp,a[i])-dp;
            if(p==temp)
            {
                dp[temp++]=a[i];
                ans=max(ans,temp);
            }
            else
                dp[p]=a[i];
        }
        if(n-k>ans)
            cout<<"A is not a magic array."<<endl;
        else
            cout<<"A is a magic array."<<endl;
    }
    }
   
2. 最长重复子序列（LCS)
3. B组B题B - Blocking Buffer 规律题<br/>
题目链接:<https://vjudge.net/contest/313274#problem/B><br/>
网上题解：<https://www.cnblogs.com/ultmaster/p/6144605.html><br/>
目测又是一道找规律题










