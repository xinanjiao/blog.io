---
layout: post
title: POJ之拓扑排序专题
date: 2020-02-18
categories: blog
tags: [拓扑排序,最小生成树,广度搜索]
description: 语言
---
<p style="color: red;font-size: 24px;">时代的一粒灰尘，落到一个人身上就是一座大山！</p>

<p style="color: red;font-size: 24px;">这不是一次死亡1000多个人的灾难，而是1000多个家庭的灾难！</p>

我们都还好，在家呆着不能出去。而有些人却只能永远呆在2020年！<br>

呆在小时候都在憧憬的2020年<br>

呆在实现小康社会的2020年<br>

呆在本该欢声笑语的2020年<br>

永远的呆在2020年。<br>

你们都是英雄！致敬<br>

希望疫情尽快结束，前线的医护人员平平安安，加油！！！

### Borg Maze POJ - 3026 最小生成树+BFS
#### 题目大意
在一个y行 x列的迷宫中，有可行走的通路空格’ ‘，不可行走的墙’$’，还有两种英文字母A和S，现在从S出发，要求用最短的路径L连接所有字母，输出这条路径L的总长度。有句话很关键。也就是：在's'点或者'A'点的时候可以分开行动，但步数另外算。
#### 思路
实际上现在想起来的话也就是把每个点到其他点的步数找出来建图，找的步骤用BFS找。然后图中找最小生成树。刚开始一直没有理解题意，导致思路错误。<br>

prim算法和kruskal都可，个人感觉这道题可能prim复杂度更低代码量更简单。<br>
建图是一门学问.<br>

坑点：<br>
图的行数和列数输入后，要用gets吃掉空格。<br>
应为输入含空格，所以最好gets输入，而且记得关掉同步流。<br>
边较多，数组开1e5才够。<br>

```

char mp[60][60];
bool book[60][60];
int number[60][60];
int dirtion[4][2]={{0,1},{0,-1},{1,0},{-1,0}};
int root[maxn];
int num,x,y,cnt;
struct edge{
    int from,to,cost;
    edge(){}
    edge(int a,int b,int c):from(a),to(b),cost(c){}
}s[maxn];
struct node{
    int x,y,step;
    node(){}
    node(int a,int b):x(a),y(b){}
}a,b,c;
bool cmp(edge a,edge b){
    return a.cost<b.cost;
}
void bfs(int u){
    mem(book,0);
    queue<node> q;
    q.push(a);
    while(!q.empty()){
        b=q.front();
        q.pop();
        if(mp[b.x][b.y]=='A'||mp[b.x][b.y]=='S'){
            s[cnt++]=edge(u,number[b.x][b.y],b.step);
        }
        for(int i=0;i<4;i++){
            int xx=b.x+dirtion[i][0];
            int yy=b.y+dirtion[i][1];
            if(xx>=0&&xx<y&&yy>=0&&yy<x&&!book[xx][yy]&&mp[xx][yy]!='$'){
                    c.x=xx,c.y=yy,c.step=b.step+1;
                    q.push(c);
                    book[xx][yy]=1;
            }
        }
    }
}
int findroot(int a){
    return root[a]==a?a:root[a]=findroot(root[a]);
}
int main()
{
   // ios::sync_with_stdio(false);
    int n;
    //cin>>n;
    scanf("%d",&n);
    while(n--){
        mem(s,0);
        mem(number,0);
        cnt=0;
       int cnt1=0;
        scanf("%d %d",&x,&y);
        mem(book,0);
        char ss[60];
        gets(ss);
        for(int i=0;i<y;i++){
            gets(mp[i]);
            for(int j=0;j<x;j++){
            //cin>>mp[i][j];
            if(mp[i][j]=='S'||mp[i][j]=='A')
                number[i][j]=cnt1++;
            }
        }
       for(int i=0;i<y;i++){
            for(int j=0;j<x;j++){
                if(mp[i][j]=='A'||mp[i][j]=='S'){
                    a.x=i,a.y=j,a.step=0;
                    bfs(number[i][j]);
                }
            }
       }
       sort(s,s+cnt,cmp);
       for(int i=0;i<=cnt;i++)
        root[i]=i;
       int sum=0;
       for(int i=0;i<cnt;i++){
        int xx=findroot(s[i].to);
        int yy=findroot(s[i].from);
        if(xx!=yy){
            sum+=s[i].cost;
            root[xx]=yy;
        }
       }
       printf("%d\n",sum);
    }
    return 0;
}
```
### Sorting It All Out POJ - 1094 拓扑排序
#### 题目大意
用小于号"<"来定义两元素之间的关系，并于一个没有重复元素的有序上升序列 从小到大地排列这些元素。
比如说，序列A,B,C,D意味着A<B;B<C;C<D。
在这个问题里，我们会给你一组形如"A<B"的关系，询问你有序序列的合法性或其本身。

#### 思路
注意啊！输入为m,n。n为待比较的字母的总数，(2<=n<=26)。m为给的限制条件，大小没告诉！！！我就觉得m最大也就!26吧。实际上m可能1e9。我老是想着开个m大小的数组存下来然后判断，所以开始一直内存超限和时间超限。<br>

还有就是我形成定势思维了，以为必须是升序，也就输输入为n=4,m=5，必须满足顺序为ABCD，确实还要看m个条件，还可以DABC呢。这就是后面为什么错了！！

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
int indu[30],n,m,indu2[30];
bool ok=0;
vector<char> a;
vector<int> s[30];
int topusort(){
    a.clear();
    priority_queue<int> p;
    for(int i=0;i<n;i++){
        if(indu2[i]==0)
            p.push(i);
    }
   // vector<char> sk;
    bool flag=0;
    int cnt=0;
    while(!p.empty()){
        if(p.size()>1)
            flag=1;
            cnt++;
        int aa=p.top();
        a.push_back('A'+aa);
        p.pop();
        for(int i=0;i<s[aa].size();i++){
            indu2[s[aa][i]]--;
            if(indu2[s[aa][i]]==0)
                p.push(s[aa][i]);
        }
    }
    //for(int i=0;i<ss.size();i++)
      //  cout<<ss[i];
    //cout<<endl;
    if(cnt!=n)
        return 0;
    else if(flag==1)
        return 1;
    else
        return 2;
}
int main()
{
    ios::sync_with_stdio(false);
    while(cin>>n>>m&&n){
        a.clear();
        for(int i=0;i<n;i++){
            s[i].clear();
            indu[i]=0;
        }
        ok=0;
        string st;
        bool flag1=0,flag2=0;
        for(int i=0;i<m;i++){
            cin>>st;
         if(flag1==0&&flag2==0){
            indu[st[2]-'A']++;
            s[st[0]-'A'].push_back(st[2]-'A');
            memcpy(indu2,indu,sizeof(indu));
            int aa=topusort();
            if(aa==0){
                cout<<"Inconsistency found after "<<i+1<<" relations."<<endl;
                ok=1;
                flag1=1;
            }
            else if(aa==2){
                cout<<"Sorted sequence determined after "<<i+1<<" relations: ";
                ok=1;
                for(int i=0;i<a.size();i++)
                    cout<<a[i];
                cout<<"."<<endl;
                flag2=1;
            }
            }
        }
        if(!ok)
            cout<<"Sorted sequence cannot be determined."<<endl;
    }
    return 0;
}
```





