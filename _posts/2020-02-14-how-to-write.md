---
layout: post
title: 2020蓝桥杯模拟赛和2019山东省赛有感
date: 2020-02-14
categories: blog
tags: [蓝桥杯,贪心]
description: 语言
---

### 感jio
最近又回顾了一下19年的山东省赛题，毕竟真题是不一样的。做起来真的感觉不一样，很有梯度，但处处是坑。不知道是不是很久没在比赛中做题了，反正签到的时候不是忘记开long long就是忘记特判，害！！马大哈一个，后面还要补补那套题当中涉及到基础算法的那几道题。比如这道H题贪心。


### H - Tokens on the Segments ZOJ - 4120 贪心

#### 题目大意
很难读懂，我懵逼了。<br>
最后的大意就是：给出n个数组对< i,j >，n<=1e5,0<=i,j<=1e9。叫我们标记每个线段，但标记的数不能重合。

#### 思路
和紫书上一道贪心题很像，从前往后贪心是错的，要排好序过后从后往前贪，且是先纵坐标再横坐标。<br>

但考虑到i，j过大，所以可能会超时。但....没有超时，也就是数据有点水。（耗时 832ms)

```
struct node{
    int x,y;
    node(){}
};
bool cmp(node a,node b){
    if(a.x==b.x)
        return a.y<b.y;
    else
       return a.x<b.x;
}
node a[maxn];
int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        mem(a,0);
        int n;
        cin>>n;
        fro(i,0,n)
        cin>>a[i].x>>a[i].y;
        sort(a,a+n,cmp);
        map<int,int> mp;
        mp.clear();
        int sum=0;
        for(int i=n-1;i>=0;i--){
            for(int j=a[i].y;j>=a[i].x;j--){
                if(mp[j]==0){
                    mp[j]=1;
                    sum++;
                    break;
                }
            }
        }
        cout<<sum<<endl;
    }
    return 0;
}
```

如果数据在线，那就不会过了，所以要用数据结构优化为O(nlogn)。<br>
采用优先队列优化<br>

也就是为了避免出现前面的把后面的标记的（尴尬）情况。一开始就把所有的坐标都放进优先队列。<br>
设定一个已经出现的最大的横坐标，让每一个队列头进行比较<br>
如果大于的话，更新最大值，计数器加一<br>
否则判断该横坐标加一是否小于等于该纵坐标。成立，横坐标加一放进优先队列。<br>
只到队列空(耗时 537ms)
注意：优先队列的cmp的写法和比较方式，因为是堆，所以要反着写！

```
struct node{
    int x,y;
    node(){}
};
struct cmp{
    bool operator()( node a, node b) {
        if(a.x==b.x)
            return a.y>b.y;
        else
            return a.x>b.x;
    }
};
node a[maxn];
int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        mem(a,0);
        int n;
        cin>>n;
        priority_queue<node,vector<node>,cmp> s;
        fro(i,0,n){
        cin>>a[i].x>>a[i].y;
        s.push(a[i]);
        }
        int sum=0,maxa=0;
        while(!s.empty()){
            node b=s.top();
            s.pop();
            if(b.x>maxa){
                maxa=b.x;
                sum++;
            }
            else
            if(b.x+1<=b.y){
                b.x++;
                s.push(b);
            }
        }
        cout<<sum<<endl;
    }
    return 0;
}
```


### 2020 蓝桥杯大学 B 组省赛模拟赛（一）

第一次做蓝桥杯的模拟题，填空题难度适中，但还是涉及：素数筛打表，dp递推，贪心策略，几何图形结论，优化策略。5道填空题就这些知识点。虽然不难但对基础还是有一定的要求，毕竟是赛后评判。所以比赛的时候必须要想到各种情况，而且后面的程序设计还挺难。需要更加仔细和思维的灵活。后面得多做类似的题来慢慢滴熟悉比赛节奏和总结应对策略。

### A. 结果填空：有趣的数字
我们称一个数是质数，而且数位中出现了 55 的数字是有趣的。例如 5, 59, 4575,59,457 都是有趣的，而 15, 715,7 不是。求 11 到 100000100000 中有趣的数的个数。

#### 解法
素数筛打表出来再判断是否含5。
	3282

### B. 结果填空：爬楼梯
蒜头君要爬楼梯。楼梯一共有 1010 层台阶。因为腿长的限制，每次最多能上 44 层台阶。但是第 5,75,7 层楼梯坏掉了不能踩。求上楼梯的方案数。

#### 解法
状态转移方程为：

	dp[i]=dp[i-1]+dp[i-2]+dp[i-3]+dp[i-4]

5，7，不能踩那就dp[5],dp[7]为0。（开始没有想到）<br>

初始化dp[1-4]，然后递推

	72

###  C. 结果填空：七巧板
求问在以下图案的大三角形内部添加五条直线最多可以将大三角形分成多少个区域。

例如下图一共有 77 个区域。

请在下图的基础上添加五条直线。

#### 解法
见链接:<https://blog.csdn.net/qq_43290288/article/details/104130718><br>
链接讲得很清楚，这是一种数学几何图形问题，有一定规律。（我是肯定想不到，狗头.jpg)

	7+6+7+8+9+10=47

###  D. 结果填空：苹果
有 3030 个篮子，每个篮子里有若干个苹果，篮子里的苹果数序列已经给出。

现在要把苹果分给小朋友们，每个小朋友要么从一个篮子里拿三个苹果，要么从相邻的三个篮子里各拿一个苹果。

苹果可以剩余，而且不同的拿法会导致不同的答案。比如对于序列3 1 3 ，可以分给两个小朋友变成0 1 0；也可以分给一个小朋友变成2 0 2，此时不能继续再分了。所以答案是 2 。

求问对于以下序列，最多分给几个小朋友？

	1 7 2 12 5 9 9 8 10 7 10 5 4 5 8 4 4 10 11 3 8 7 8 3 2 1 6 3 9 7 1

#### 解法
可以尝试三种贪心思路：比如先拿3个的倍数，再最后全部拿一。反过来。第一次取3个苹果，第二次取1 1 1个苹果（只取一次）。最后取最优

	62

### E. 结果填空：方阵 （暂时保留）


###  F. 程序设计：寻找重复项
有一个数列{an}，a0为1，ai+1=(A* ai+aimodB)modC，请你编程求出这个数列第一次出现重复的项的标号。如果答案超过 2000000 输出 "-1"（不加引号）

#### 解法
模拟即可，值得注意的是。判断出现过的数的时候，如果用map会超时，因为紫书上也说过:当常数项过大后，map也会超时。所以后面我手写hash表就过了。听说unoder map都行，没试过。

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
const int maxn = 1e5+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
struct hash_map{
    static const int maxa=0x7fffff;
    int q[hashmaxn],p[hashmaxn];
    void clear(){
        for(int i=0;i<=maxa;i++)
            q[i]=0;
    }
    int &operator[](int k){
        int i;
        for(i=k&maxa;q[i]&&p[i]!=k;i=(i+1)%maxa);
        p[i]=k;
        return q[i];
    }
}ss;
int main(){
    ios::sync_with_stdio(0);
    ll a,b,c;
    cin>>a>>b>>c;
    map<ll,ll> mp;
    mp[1]=1;
    ll sum=1,pos=0,ans;
    bool flag=0;
    ss.clear();
    ss[1]++;
    while(1){
        sum=(a*sum+sum%b)%c;
        pos++;
        if(pos>2000000){
            cout<<-1<<endl;
            break;
        }
        if(ss[sum]){
            cout<<pos<<endl;
            break;
        }
            ss[sum]++;
    }
    return 0;
}
```

未完待续.....