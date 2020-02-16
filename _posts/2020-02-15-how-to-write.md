---
layout: post
title: 线性结构上的动态规划 uva 11400
date: 2020-02-15
categories: blog
tags: [动态规划]
description: 语言
---

### 线性结构上的动态规划
著名的问题有最长上升子序列(LIS)，最长公共子序列(LCS)。关于状态转移我也在暑假熟悉了一下，但没有细想，借这个机会，结合网上的各种资料对这两个问题理解得更透彻了。<br>

LCS问题，可以输出最长公共序列之一，借助递归，想想dp[i][j]那个二维矩阵图好理解点。和01背包差不多应该，反正递归判断dp[i-1][j-1],dp[i-1][j]和dp[i][j-1]。<br>

LIS问题，复杂度为O(n^2)，且无法输出路径（最长序列）。在数据结构的优化下复杂度可以降为O(nlogn)。<br>

还有很多此类问题有待解决。<br>

### Lighting System Design UVA - 11400
#### 题目大意
有一个照明系统需要用到n种灯，每种灯的电压为V，电源费用K，每个灯泡费用为C，需要该灯的数量为L。注意到，电压相同的灯泡只需要共享一个对应的电源即可，还有电压低的灯泡可以被电压高的灯泡替代。为了节约成本，你将设计一种系统，使之最便宜。

#### 思考
首先需要明确一种灯泡要么全部换，要么不换。如果换一部分的话，首先电源费用得不到节约，那么节约的部分就只来自于换的那部分灯泡，既然可以节约钱干嘛不干脆全部换了呢? 
明确了这点之后 <br>
我们应该对决策进行一定的限制: 求前i个灯泡的最小花费时，只允许用第i种灯泡进行替换! 如何替换呢？ 只能替换1 到i 中序号连续的灯泡！！！ 即决策应为选择一个j，替换掉序号为 j 到i - 1的所有灯泡，使得前i号灯泡花费最小。那么最终 dp[n]即为答案。 
d[i]为只考虑前i种灯泡的最小花费，状态转移方程：**d[i] = min{d[i] + (s[i] - s[j]) * c[i] + k[i]} 其中s[i]为前i种灯泡的总数量**。<br>

以上就是网上大佬的一个讲解。对于我这种入门小白来说，把这个状态转移方程写出来是很费劲的。现在暂时停留在理解方面。<br>

我的理解：该问题的子问题就是dp[i]，前i个的最优方案，n个决策，也就是，从0到i-1，尝试各种替换方法。使用dp[j]也就是前j个用最优方案,后j+1--i-1个用自身。因为将伏特从大到小排序后，要求只能伏特大的替换小的。每种方案都互相覆盖，不会漏解。<br>

```
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <fstream>
#include <algorithm>
#include <cmath>
#include <deque>
#include <vector>
#include <queue>
#include <string>
#include <cstring>
#include <map>
#include<time.h>
#include <stack>
#include <list>
#include <set>
#include <sstream>
#include <iterator>
using namespace std;
#define FOPI freopen("codecoder.in", "r", stdin)
#define DOPI freopen("codecoder.out", "w", stdout)
#define ll long long int
#define fro(i,a,n) for(ll i=a;i<n;i++)
#define pre(i,a,n) for(ll i=n-1;i>=a;i--)
#define mem(a,b) memset(a,b,sizeof(a))
#define ls l,mid,rt<<1
#define rs mid+1,r,rt<<1|1
#define fi first
#define se second
typedef pair<ll,ll> P;
ll gcd(ll a,ll b){return b==0?a:gcd(b,a%b);}
const double PI = 3.1415926535897932;
const double EPS=1e-10;
const int INF=0x3f3f3f3f;
const int maxn = 1e3+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
struct node{
    int v,k,c,l;
    node(){}
}a[maxn];
bool cmp(node a,node b){
    return a.v<b.v;
}
int dp[maxn],sum[maxn];
int main()
{
    ios::sync_with_stdio(false);
    int n;
    while(cin>>n&&n){
        fro(i,1,n+1)
        cin>>a[i].v>>a[i].k>>a[i].c>>a[i].l;
        sort(a+1,a+n+1,cmp);
        mem(dp,0);mem(sum,0);
        sum[1]=a[1].l;
        for(int i=2;i<=n;i++) sum[i]=sum[i-1]+a[i].l;
        for(int i=1;i<=n;i++){
            dp[i]=sum[i]*a[i].c+a[i].k;//全部用做第i种
            for(int j=1;j<i;j++){
                dp[i]=min(dp[i],dp[j]+(sum[i]-sum[j])*a[i].c+a[i].k);//前j种用最优，j+1道i种用自身
            }
        }
        cout<<dp[n]<<endl;
    }
    return 0;
}
```












