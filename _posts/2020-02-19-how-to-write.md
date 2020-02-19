---
layout: post
title: POJ专题之二分图的最大匹配
date: 2020-02-19
categories: blog
tags: [二分图最大匹配]
description: 语言
---

### 最大二分图匹配

这里介绍匈牙利算法。在前面的博客介绍过，那就不在重复了。<https://zhengzihao.online/blog/2019/08/19/how-to-write/><br>

原来做的这方面的题都很简单，已经给出二分关系图，直接上算法就可以搞定的，不需要建图那些。<br>
今天的题就涉及到额外的知识。

## 最小点覆盖 最小路径覆盖

### 最小点覆盖
点覆盖的概念定义： 
对于图G=(V,E)中的一个点覆盖是一个集合S⊆V使得每一条边至少有一个端点在S中。

最小点覆盖：用点来覆盖边的最小个数
普通图的最小点覆盖数好像只能用搜索解，没有什么比较好的方法（可能我比较弱。。）所以在此只讨论二分图的最小点覆盖的求法

结论： **二分图的最小点覆盖数=该二分图的最大匹配数**

## 最小路径覆盖

### DAG上的不相交最小路径覆盖
定义：在一个有向图中，找出最少的路径，使得这些路径经过了所有的点。

最小路径覆盖分为最小不相交路径覆盖和最小可相交路径覆盖。

最小不相交路径覆盖：每一条路径经过的顶点各不相同。如图，其最小路径覆盖数为3。即1->3>4，2，5。

最小可相交路径覆盖：每一条路径经过的顶点可以相同。如果其最小路径覆盖数为2。即1->3->4，2->3>5。

特别的，每个点自己也可以称为是路径覆盖，只不过路径的长度是0。

结论：**最小路径覆盖数 = 节点数 - 最大匹配数**

### DAG上的相交最小路径覆盖
算法：把原图的每个点V拆成VxVx和VyVy两个点，如果有一条有向边A->B，那么就加边Ax−>ByAx−>By。这样就得到了一个二分图。那么**最小路径覆盖=原图的结点数-新图的最大匹配数**。

证明：一开始每个点都是独立的为一条路径，总共有n条不相交路径。我们每次在二分图里找一条匹配边就相当于把两条路径合成了一条路径，也就相当于路径数减少了1。所以找到了几条匹配边，路径数就减少了多少。所以有**最小路径覆盖=原图的结点数-新图的最大匹配数**。

先将图用floyd算法跑一遍，合并重复边，然后建立二分图，最后跑最大匹配。

###无向图的最小路径覆盖
建图和上面一样，但最后的最小路径数为:<br>
**最小路径数=顶点数-最大匹配数/2**


图论的题，建好图就可了，建图是难点，有时看数据小就会被误导到暴力去，得多做题体会体会！!

### Asteroids POJ - 3041 最小点覆盖
####题目大意
给一个N* N的矩阵，有些格子有障碍，要求我们消除这些障碍，问每次消除一行或一列的障碍，

最少要几次。
### 思路
不知道最小点覆盖的话，虽然我知道是二分图匹配也不知道怎样建图。<br>
想想也就是将行看成二分图的一边，列看为另外一边。<br>
按这给关系将给出的坐标建图，。一次性可以消掉一列或一行。<br>
现在的问题是 我们要在新图中选择最少的点使得所有边都至少有一个端点被选中了。这就是最小点覆盖问题！

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
int n,m;
bool match[510][510];
bool book[510];
int used[510];
bool dfs(int a){
    for(int i=1;i<=n;i++){
        if(!book[i]&&match[a][i]){
            book[i]=1;
            if(!used[i]||dfs(used[i])){
                    used[i]=a;
                return true;
            }
        }
    }
    return false;
}
int main()
{
    ios::sync_with_stdio(false);
    cin>>n>>m;
    for(int i=0;i<m;i++){
        int x,y;
        cin>>x>>y;
        match[x][y]=1;
    }
    int sum=0;
    for(int i=1;i<=n;i++){
        mem(book,0);
        if(dfs(i))
            sum++;
    }
    cout<<sum<<endl;
    return 0;
}
```

### Antenna Placement POJ - 3020 无向图的最小路径覆盖
#### 题目大意
有一张1 * 2大小的纸片，问需要覆盖所有标记的点的最小纸片数。

#### 思路
首先要覆盖所有的点，所以就是求最小的边让所有点覆盖，也就是最小路径覆盖（看看定义）。<br>
这里给出的是一张地图，还要根据地图建图。建完的二分图是无向图。<br>
建图方法就是如果纸片能覆盖住四个方向的点，就加一条边！<br>
在匈牙利算法的used数组那里，把它定义为了bool类型错了很多次！！<br>



	

