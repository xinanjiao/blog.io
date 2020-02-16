---
layout: post
title: POJ之最短路专题 
date: 2020-02-16
categories: blog
tags: [Dijsktra,Bellman-ford,Floyd]
description: 语言
---
### 最短路径
保留一个poj专题训练的题单，本来在上学的时候就开始刷的，然后考试临近，然后就没有刷了，无意间看见了这个题单，打算边复习前面学的算法边做题。<br>

最短路径算法我也忘了很多，就连dijsktra我也忘了，所以回顾还是有必要的！<br>

这个专题有6道题，基本都是题意晦涩难懂，诱导我们往难的地方思考，实际上都是简单的最短路问题，bellman-ford算法求最短路里的负环回路的同时还可以求正环回路。floyd也可以判读负环，也可用做求传递闭包。dijsktra重点复习一下，n^2的算法，也可以用数据结构优化为nlogn。<br>

求最短路中，建图也是一项本领，建图建得好，那就是事半功倍！

## bellman-ford算法求正（负）环

### Currency Exchange POJ - 1860 
#### 题目大意
就看能不能在现有的金钱前提下，通过汇率调换钞票，最后达到增值的过程。

#### 思路
bellman-ford求正环。<br>
这个算法复杂度为n* m，即边数乘以点数，只要在n-1次的边松弛中还存在松弛，那就存在环！<br>
给定起始钞票类型及金额。

```
struct edge
{
    int from,to;
    double cost,pay;
    edge(){}
    edge(int a,int d,double b,double c):from(a),to(d),cost(b),pay(c){}
}s[maxn];
double dis[maxn];
int cnt;
int main()
{
    ios::sync_with_stdio(0);
    int v,adj,start;
    double money;
    cin>>v>>adj>>start>>money;
    int a,b;
    double c,d,e,f;
    fro(i,0,adj)
    {
        cin>>a>>b>>c>>d>>e>>f;
        s[++cnt]=edge(a,b,c,d);
        s[++cnt]=edge(b,a,e,f);
    }
    mem(dis,0);
    dis[start]=money;
    bool ok=0;
    fro(j,1,v+1)
    {
        fro(i,1,cnt+1)
        if(dis[s[i].to]<(dis[s[i].from]-s[i].pay)*s[i].cost)
        {
            dis[s[i].to]=(dis[s[i].from]-s[i].pay)*s[i].cost;
            if(j==v)
              {
                  ok=1;
              }
        }
    }
    if(ok)
        cout<<"YES"<<endl;
    else
        cout<<"NO"<<endl;
    return 0;
}
```

### Arbitrage POJ - 2240
#### 题目大意
同上一道题，不同的是，其实钞票没告诉及金额没告诉。

#### 思路
同上分析，只不过需要将每个钞票类型作为起点跑一次bellman-ford

```
double dis[maxn];
struct edge{
    int to,from;
    double cost;
    edge(){}
    edge(int a,int b,double c):to(a),from(b),cost(c){}
}s[maxn];
int main()
{
    ios::sync_with_stdio(false);
    int n,case1=1;
    while(cin>>n&&n){
        map<string,int> mpp;
        int cnt=1;
        for(int i=0;i<n;i++){
            string s;
            cin>>s;
            mpp[s]=cnt++;
            //cout<<mpp[s]<<endl;
        }
        int m;
        cin>>m;
        for(int i=0;i<m;i++){
            string a,b;
            double ss;
            cin>>a>>ss>>b;
            s[i]=edge(mpp[a],mpp[b],ss);
        }
        bool ok=0;
        for(int i=1;i<=n;i++){
            mem(dis,0);
            dis[1]=10;
            for(int j=1;j<=n;j++){
                for(int k=0;k<m;k++){
                        if(dis[s[k].from]<dis[s[k].to]*s[k].cost){
                            dis[s[k].from]=dis[s[k].to]*s[k].cost;
                            if(j==n){
                                ok=1;
                            }
                    }
                }
            }
            //fro(i,1,n+1)
            //cout<<dis[i]<<" ";
        }
        cout<<"Case "<<case1++<<": ";
        if(ok)
            cout<<"Yes"<<endl;
        else
            cout<<"No"<<endl;
    }
    return 0;
}
```

## Floyd 算法求最短路

### Frogger POJ - 2253
湖中有n块石头，编号从1到n，有两只青蛙，Bob在1号石头上，Alice在2号石头上，Bob想去看望Alice，但由于水很脏，他想避免游泳，于是跳着去找她。但是Alice的石头超出了他的跳跃范围。因此，Bob使用其他石头作为中间站，通过一系列的小跳跃到达她。两块石头之间的青蛙距离被定义为两块石头之间所有可能路径上的最小必要跳跃距离，某条路径的必要跳跃距离即这条路径中单次跳跃的最远跳跃距离。你的工作是计算Alice和Bob石头之间的青蛙距离


### 思路
考虑其他每个石头都可以跳，即floyd算法。注意**某条路径的必要跳跃距离即这条路径中单次跳跃的最远跳跃距离**。
需事先获得两两之间的距离。<br>
G++输出浮点数用".f"

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
double mp[1100][1100];
struct node{
    double x,y;
    node(){}
}a[210];
double getdis(node a,node b){
    return sqrt((a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y));
}
int main()
{
    ios::sync_with_stdio(false);
    int n,case1=1;
    while(cin>>n&&n){
        fro(i,1,n+1)
        cin>>a[i].x>>a[i].y;
        if(n==2)
            mp[1][2]=getdis(a[1],a[2]);
        else{
            fro(i,1,n+1) fro(j,1,n+1) if(i!=j) mp[i][j]=getdis(a[i],a[j]); else mp[i][j]=0;

        fro(k,1,n+1){
            fro(i,1,n+1){
                fro(j,1,n+1){
                    
                    mp[i][j]=min(mp[i][j],max(mp[i][k],mp[k][j]));
                }
            }
        }
        }
        cout<<"Scenario #"<<case1++<<endl;
        printf("Frog Distance = ");
        printf("%.3lf\n",mp[1][2]);
        cout<<endl;
    }
    return 0;
}
```

## dijsktra 算法求最短路 经典题目
### Stockbroker Grapevine POJ - 1125

#### 题目大意
三角洲的消息不知为何泄露了出去。间谍对传递的信息十分敏感。现在你被雇佣去开发一种在间谍之间传播虚假信息的程序，以次来保护各个领导人的安全。为了获得最大的效果，你必须在尽可能快的时间内传播谣言。
不幸的是，间谍们只信赖来自他们认为是“可靠来源”的消息。这意味着你必须在开始传播流言时考虑他们之间的关系。当流言开始传播时，某个间谍需要一定的时间将其传递给他的所有线人。
你的任务是编写一个程序，输出需要选择哪个间谍作为流言传播的起点，以及这个流言传播完整个间谍圈所需的时间。所需的时间指的是最后一个间谍接受到消息所花费的总时间<br>

#### input
你的程序将输入多个不同间谍群体的数据。每一组的第一行是间谍的人数。接下来一行包括每个间谍可以联系的线人的数量，这些人是谁，和他传递信息给每一个人所花的时间。每一行格式如下：最开始是可以联系的人的数目 n，然后是 n对整数，一对整数代表了他与一个联系人的情况。每一对整数列出的第1个数字是联系人编号（例如："1"是指社群中的1号联系人），第2个数字是指把消息传给那个联系人需要花几分钟。没有其他的标点符号或空格。 
每个人的编号为i(1 ≤ i ≤ n, n为一个社群中间谍的总数量），传递信息的时间为t分钟（1 ≤ t ≤ 10），可以与之联系的人的数量为x （0 ≤ x ≤ n-1），间谍的数量为n(1 ≤ n ≤ 100) 。输入的终止条件是间谍社群仅有0个人。  

#### output
对于每一组数据，你的程序必须输出一行整数，包含能使消息传递得最快的那个联系人，以及给定的消息从这个人传递到最后一个人所花费的时间，以整数分钟来度量。 
你的程序可能会收到某种排除了一些人在外的联系网络，如有些人可能无法被任何人联系到。如果你的程序检测到这种不连通的网络，只需输出“disjoint”。请注意，如果消息既能从A传递到B，又能从B传递到A，则两个传递消息的时间不一定相同

#### sample input
```
3
2 2 4 3 5
2 1 2 3 6
2 1 2 2 2
5
3 4 4 2 8 5 3
1 5 8
4 1 6 4 10 2 7 5 2
0
2 2 5 1 5
0
```
#### sample output
```
3 2
3 10
```
#### 思路
题目题难理解的，特别是英文的题面。<br>
刚开始想得很复杂。实际上就是把每一个点作为起点跑dijsktra，然后再最短路径中找最大的时间，然后每个进行比较得到最小的时间及起点。

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
const int maxn = 1e2+10;
const int hashmaxn=8388608;
int lowbit(int x){return x&(-x);}
bool book[maxn];
vector<int> s[maxn];
int mp[maxn][maxn];
int dis[maxn];
int n;
void dijsktra(int a){
    mem(book,0);
    book[a]=1;
    mem(dis,INF);
    dis[a]=0;
    for(int i=0;i<s[a].size();i++)
        dis[s[a][i]]=mp[a][s[a][i]];
    fro(i,0,n){
        int mina=INF,u=0;
        for(int j=1;j<=n;j++){
            if(!book[j]&&dis[j]<mina){
                mina=dis[j];
                u=j;
            }
        }
        book[u]=1;
        for(int i=0;i<s[u].size();i++){
            int a=s[u][i];
            if(!book[a])
            dis[a]=min(dis[a],dis[u]+mp[u][a]);
        }
    }
}
int main()
{
    ios::sync_with_stdio(false);
    while(cin>>n&&n){
        mem(mp,0);
        for(int i=1;i<=n;i++)
            s[i].clear();
        for(int i=1;i<=n;i++){
            int t;
            cin>>t;
            fro(j,0,t){
                int a,b;
                cin>>a>>b;
                s[i].push_back(a);
                mp[i][a]=b;
            }
        }
        int maxa=INF,pos=-1;
        for(int i=1;i<=n;i++){
            dijsktra(i);
            int temp=-1;
            for(int j=1;j<=n;j++){
                if(dis[j]>temp){
                    temp=dis[j];
                }
            }
            if(temp<maxa){
                maxa=temp;
                pos=i;
            }
        }
        if(pos==-1)
            cout<<"disjoint"<<endl;
        else
            cout<<pos<<" "<<maxa<<endl;
    }
    return 0;
}
```





