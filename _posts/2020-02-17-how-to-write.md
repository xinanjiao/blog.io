---
layout: post
title: POJ之最小生成树专题
date: 2020-02-17
categories: blog
tags: [最小生成树]
description: 语言
---

### 最小生成树
最小生成树最广为人知的两个算法：**Prim**和**Kruskal**。<br>
我个人对Kruskal比较偏好，因为认识较早。但两者各有局限，在不同数据限制下虽然可以相互转换，但在苛刻的条件下有时候只能用其中之一。所以需要两个都同时掌握。

### Kruskal算法
库鲁斯卡尔算法简要分析一波。<br>
1. 建图
2. 按边的大小排序
3. 选边，同时运用并查集看是否会构成环

因为是按边来选择，所以复杂度与边有关，所以适用于稀疏图。复杂度：O(nlogn)，n为边的条数。

### Prim算法
普尼姆算法简要分析。<br>
1. 建图
2. 任意选一个点出发，开始对每一条边‘松弛’，次数为n-1次。
3. 也就是在现在的dis数组中找出最小的边，然后借这个点更新dis数组，最后更新n-1次。

因为选点作为参考，所以复杂度与点的个数有关，适用于稠密图。复杂度为：O(n^2)。n为点的个数。


### POJ 题目


### Truck History POJ - 1789
#### 题目大意
给定N个字符串，某个字符串转为另一个字符串的花费为他们每一位不相同的字符数。 求最小花费Q。


#### 思路
光看英文题面，可能会被带到动态规划方面。但基于每一个字符串都由上一个得到。所以连起来就是一张图。而且是一个完全连通图。也就是该顶点与其余n-1个点都相连。那就是稠密图，那最好就用Prim算法。输出格式错误，白给一波。

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
const int maxn = 2e3+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
int dis[maxn],n;
int mp[maxn][maxn];
bool book[maxn];
string s[maxn];
int sum1;
void prim(){
    for(int i=1;i<n;i++){
        int mina=INF,pos;
        for(int j=1;j<=n;j++){
            if(!book[j]&&dis[j]<mina){
                mina=dis[j];
                pos=j;
            }
        }
        sum1+=mina;
        book[pos]=1;
        for(int k=1;k<=n;k++){
            if(!book[k]&&dis[k]>mp[pos][k])
                dis[k]=mp[pos][k];
        }
    }
}
int main()
{
    ios::sync_with_stdio(false);
    while(cin>>n&&n){
        mem(dis,INF);mem(mp,0);mem(book,0);
        sum1=0;
        for(int i=1;i<=n;i++)
            cin>>s[i];
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(i==j) mp[i][j]=0;
                else{
                    int sum=0;
                    for(int k=0;k<7;k++){
                        if(s[i][k]!=s[j][k])
                            sum++;
                    }
                    mp[i][j]=sum;
                }
            }
        }
        for(int i=2;i<=n;i++)
            dis[i]=mp[1][i];
        //dis[1]=0;
        book[1]=1;
        prim();
        cout<<"The highest possible quality is "<<"1/"<<sum1<<"."<<endl;
    }
    return 0;
}
```

### Highways POJ - 2485
#### 题目大意
A国没有高速公路，因此A国的交通很困难。政府意识到了这个问题并且计划建造一些高速公路，以至于可以在不离开高速公路的情况下在任意两座城镇之间行驶。

A国的城镇编号为1到N, 每条高速公路连接这两个城镇，所有高速公路都可以在两个方向上使用。高速公路可以自由的相互交叉。

A国政府希望尽量减少最长高速公路的建设时间（使建设的最长的高速公路最短），但是他们要保证每个城镇都可以通过高速公路到达任意一座城镇。

#### 思路
中规中矩的最小生成树题目，读完就可以联想到最小生成树。最后的输出就是最小生成树路径上最大的边。用Kruskal算法，实际上两者都可

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
int n;
const int xx=2e5+10;
int root[maxn];
int mp[maxn][maxn];
struct edge{
    int from,to,cost;
    edge(){}
    edge(int a,int b,int c):from(a),to(b),cost(c){}
}a[xx];
int findroot(int a){
    return a==root[a]?a:root[a]=findroot(root[a]);
}
bool cmp(edge a,edge b){
    return a.cost<b.cost;
}
int main()
{
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--){
        cin>>n;
        int minlen=-1;
        mem(mp,0);mem(a,0);mem(root,0);
        for(int i=1;i<=n;i++)
            root[i]=i;
        int cnt=1;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                cin>>mp[i][j];
            }
        }
        //int ans=n*(n-1)/2;
        for(int i=1;i<=n;i++){
            for(int j=i+1;j<=n;j++){
                a[cnt++]=edge(i,j,mp[i][j]);
            }
        }
        sort(a+1,a+cnt,cmp);
        for(int i=1;i<cnt;i++){
            edge s=a[i];
            //cout<<s.from<<" "<<s.to<<" "<<s.cost<<endl;
            int x=findroot(s.from);
            int y=findroot(s.to);
            //cout<<x<<" "<<y<<endl;
            if(x!=y){
                root[y]=x;
                minlen=max(minlen,s.cost);
            }
        }
        cout<<minlen<<endl;
    }
    return 0;
}
```

### Agri-Net POJ - 1258 

和上面道题差不多，改改代码就可以过。






