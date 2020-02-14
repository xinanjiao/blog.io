---
layout: post
title: 01背包问题(输出路径) uva 12563+624
date: 2020-02-13
categories: blog
tags: [动态规划]
description: 语言
---

### 01背包
01背包在暑假系统的看过，但没有认真的去理解，只记了状态转移公式。在紫书上的动态规划专题上有遇见了，加上前面的DAG问题，TSP问题，和多阶段多决策问题的铺垫，在对动态规划有一定了解情况下再去理解起来更加容易了，加之网上视频的学习，也略知一二了吧。<br>

1. 阶段
2. 决策
3. 转移策略
4. 转移方程

但01背包的问题还有很多变形，还得继续研究。<br>

<p style="color: red">如果dp[i][j]看做第i岁的时候一个人的收获是j，整个一生就是最优决策问题，如何在有限的一生取得最大的价值。这也是一个dp问题....(做好每一个决策，只取最优，那最后的结果必定最优）</p>

### Jin Ge Jin Qu hao UVA - 12563 01背包问题

#### 题目大意
 KTV里面有n首歌曲你可以选择,每首歌曲的时长都给出了. 对于每首歌曲,你最多只能唱1遍. 现在给你一个时间限制t (t<=10^9) , 问你在最多t-1秒的时间内可以唱多少首歌曲num , 且最长唱歌时间是多少time (time必须<=t-1) ? 最终输出num+1 和 time+678 即可.

       注意: 你需要优先让歌曲数目最大的情况下,再去选择总时长最长的.

#### 思路
t最多不会超过50 * 180+678。要满足在唱歌最多的情况下还要唱的时间最多。你们我们至少要给一秒来满足唱最后那首11分钟的歌。因为本题是两个最优解:数量最优，且时长最优。那我们可以设置两个状态转移方程。

	dp[i][j]=max(dp[i+1][j],dp[i+1][j-v[i]]+1)

其中dp[i][j]表示当还有时长为j的时候，在前i个选的最大歌数量。

	times[i][j]=max(times[i+1][j],dp[i+1][j-v[i]]+v[i])

和上面数组含义差不多，但是times的更新值是在数量最多的条件下更新，且取最优！


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
const int maxn = 1e4+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
int dp[100][maxn];
int value[100][maxn];
int v[100];
int n,t;
int main(){
    ios::sync_with_stdio(0);
    int tt;
    cin>>tt;
    int case1=1;
    while(tt--){
        cin>>n>>t;
        mem(dp,0),mem(value,0);
        fro(i,0,n)
        cin>>v[i];
       // sort(v,v+n,greater<int>());
        int c=t-1;
        for(int i=n-1;i>=0;i--){
            for(int j=0;j<=c;j++){
                dp[i][j]=i==n-1?0:dp[i+1][j];
                value[i][j]=i==n-1?0:value[i+1][j];
                if(j>=v[i]){
                    if(dp[i][j]<dp[i+1][j-v[i]]+1){
                        dp[i][j]=dp[i+1][j-v[i]]+1;
                        value[i][j]=value[i+1][j-v[i]]+v[i];
                    }
                    else if(dp[i][j]==dp[i+1][j-v[i]]+1){
                        value[i][j]=max(value[i][j],value[i+1][j-v[i]]+v[i]);
                    }
                }
            }
        }
        cout<<"Case "<<case1++<<": ";
        cout<<dp[0][c]+1<<" ";
        cout<<value[0][c]+678<<endl;
    }
    return 0;
}

```

### CD UVA - 624 01背包路径打印

#### 题目大意
01背包问题的标准问法，只不过最后叫你打印出装的哪些物品

#### 思路
这也是一个01背包问题路径打印的模板，因为是二维数组，所以路径就返回去找就行了，而且本题要求是正推，正推的话，数组开始必须是1，不能是0！！！记住


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
const int maxn = 1e4+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
int dp[50][maxn];
//int path[50][maxn];
int v[50];
int n,m;
void printpath(int a,int b){//打印路径
    if(a==0)
        return ;
    if(dp[a][b]==dp[a-1][b])
        printpath(a-1,b);
    else{
        printpath(a-1,b-v[a]);
        cout<<v[a]<<' ';
    }
}
int main(){
    ios::sync_with_stdio(0);

    while(cin>>n){
        mem(dp,0);mem(v,0);
        //mem(path,0);
        cin>>m;
        fro(i,1,m+1){
            cin>>v[i];
        }
        for(int i=1;i<=m;i++){
            for(int j=0;j<=n;j++){
                dp[i][j]=(i==1?0:dp[i-1][j]);
                if(j>=v[i])
                    dp[i][j]=max(dp[i][j],dp[i-1][j-v[i]]+v[i]);
            }
        }
        printpath(m,n);
        cout<<"sum:"<<dp[m][n]<<endl;

    }
    return 0;
}
```




