---
layout: post
title: UVA 161  多阶段决策问题
date: 2020-02-10
categories: blog
tags: [动态规划]
description: 语言
---

### Unidirectional TSP UVA - 116 动态规划

#### 题目大意
给定m行n列（m<=10,n<=100）的整数矩阵，从第一列的任何一个位置出发每次往右或右上或右下走一格，最终到达最后一列。要求经过的整数之和最小。整个矩阵是环形的，即 第一行的上一行是最后一行，最后一行 的下一行是第一行。输出路径上的每列的行号。多解时输出字典序最小的

#### 分析
紫书上说这是一种特殊的DAG，因为可以认为‘图里还有图’。直白一点就是当前阶段影响着下一个阶段，而每一个阶段都有不同的决策，且不同阶段互相影响，也就是多阶段决策问题。<br>

这道题和数字金字塔很像哈，实际上就差不多，分为多个阶段，每个阶段的决策为向右，右上，右下。<br>

所以状态转移方程dp[i][j]可顺推也可逆推。在这里先逆推（后面说原因）。<br>

设dp[i][j]为**以i行，j列开始的点花费的最短距离**<br>

所以状态转移方程为：**dp[i][j]=min(dp[i/i+1/i-1][j+1]+v[i][j])**

注意边界当j为n-1时。<br>

现在考虑字典序最小问题：<br>

参考了一部分资料。结论是：<br>

<p style="color: red">顺推能保证下一个最小，逆推能保证前一个最小</p>

有点迷。。。。有待理解。<br>

记录的方式就是nextpos[i][j]数组，表示第i行j列那个位置的下一个位置的行，应为列总是加一。<br>

所以代码就出来了。

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
const int maxn = 1e9+7;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
int dp[110][110];
int mp[110][110];
int nextpos[110][110];
int m,n;
void print(int a){
    cout<<a+1;
    int pos=0;
    while(pos<n-1){
        int aa=nextpos[a][pos];
        cout<<" "<<aa+1;
        a=aa;
        pos++;
    }
}
int main(){
    ios::sync_with_stdio(0);

    while(cin>>m>>n){
        mem(dp,INF);mem(mp,0);mem(nextpos,INF);
        fro(i,0,m)
          fro(j,0,n)
          cin>>mp[i][j];
          int ans=INF;
        //dp[i][j] 从i行j列结束到最后一列的最优选择
        for(int j=n-1;j>=0;j--){//列
            for(int i=0;i<m;i++){//行
                    if(j==n-1){
                        dp[i][j]=mp[i][j];
                    }
                    else{
                        int s[3]={i,i-1,i+1};
                        if(i==0)s[1]=m-1;
                        if(i==m-1) s[2]=0;
                        sort(s,s+3);
                        for(int k=0;k<3;k++){
                            int now=dp[s[k]][j+1]+mp[i][j];
                            if(now<dp[i][j]){
                                dp[i][j]=now;
                                nextpos[i][j]=s[k];
                                //cout<<i<<"行"<<j<<"列"<<"下一个为"<<" "<<nextpos[i][j]<<endl;
                            }
                        }
                    }
                if(j==0)
                ans=min(ans,dp[i][j]);
            }
            }
            int pos=0;
            for(int i=0;i<m;i++){
                if(dp[i][0]==ans){
                    pos=i;
                    break;
                }
            }
            print(pos);
            cout<<endl<<ans<<endl;
        }
    return 0;
}
```












