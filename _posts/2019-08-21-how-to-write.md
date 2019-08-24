---
layout: post
title: ACM训练第23天（计算几何）
date: 2019-8-21
categories: blog
tags: [算法,训练,计算几何]
description: 语言
---

### 写在前面
今天是我的锅，两道题因为想错了算法而止步不前，唉，把floyld求闭包想成了并查集，awsl，真的，今天有点怄气，题做不动，还把船聊没了（私事）。计算几何杀我，可能还是菜鸡，初次接触此类题，大眼瞪小眼，涉及了计算几何方面的题，所以不知道也是正常，慢慢来嘛，不能步，怎能run。

### 题目
今天呢，题目涉及foyld算法求闭包，dp动态规划递推求解，另外就是杀我的计算几何之叉积求点与线段之间的关系。<br/>



### Problem D Pants On Fire floyld算法求闭包
题目链接：<https://codeforces.com/gym/101873/attachments/download/7413/20172018-acmicpc-german-collegiate-programming-contest-gcpc-2017-en.pdf><br/>
知道题目算法的我眼泪掉下来，并查集写了一发，以wa test 3结尾，这个就在告诉你算法不对头，实际上我们都商量出来算法问题，但苦于不知道上面算法而告终，事后自敲一发传递闭包
AC。

    typedef pair<int,int> P;
    ll gcd(ll a,ll b) {return b?gcd(b,a%b):a;}
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn = 3e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    cin>>m>>n;
    string a,b,c,d,e;
    map<string ,int> ss;
    int count1=1;
    int dis[201][201];
    for(int i=0;i<m;i++)
    {
       cin>>a>>b>>c>>d>>e;
       if(ss[a]==0)
        ss[a]=count1++;
       if(ss[e]==0)
        ss[e]=count1++;
        dis[ss[a]][ss[e]]=1;
    }
    for(int k=0;k<count1;k++)
    {
       for(int i=0;i<count1;i++)
       {
           for(int j=0;j<count1;j++)
           {
               if(dis[i][k]&&dis[k][j])
                dis[i][j]=1;
           }
       }
    }
    for(int i=0;i<n;i++)
    {
       cin>>a>>b>>c>>d>>e;
       if(ss[a]==0||ss[e]==0)
        puts("Pants on Fire");
       else
       {
           if(dis[ss[a]][ss[e]])
            puts("Fact");
            else if(dis[ss[e]][ss[a]])
            puts("Alternative Fact");
            else
            puts("Pants on Fire");
       }
    }
    return 0;
    }

### Problem I Überwatch dp动态规划递推
题目链接：<https://codeforces.com/gym/101873/attachments><br/>
做的时候队友一直建议深搜写，实际上这是不可能的，超时是最大的障碍，300000的数据，深搜是不可能了，这时又没有好的算法策略而停滞不前，事后（又是事后。。。），知道dp后，用我们原来深搜的思想推出了dp的状态转移方程，一发失败，因为第一次是m+1那个点开始，第二发AC。

     #include <bits/stdc++.h>
     using namespace std;
    const int maxn = 3e5+10;
    const int INF=0x3f3f3f3f;
    int d[maxn];
    int vis[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    cin>>m>>n;
    for(int i=1;i<=m;i++)
        cin>>vis[i];
    memset(d,0,sizeof(d));
    for(int i=n+1;i<=m;i++)
    {
        d[i]=max(d[i-1],d[i-n]+vis[i]);
    }
        cout<<d[m]<<endl;
    return 0;
    }

### A - TOYS 计算几何---二分+叉积
题目链接：<https://vjudge.net/contest/321263#problem/A><br/>
涉及计算几何关于线段与点位置关系的基础知识。看链接快速了解。<br/>
点与直线的位置关系，传送门：<https://blog.csdn.net/qiushangren/article/details/90475707><br/>
向量积居然能怎么用，我是长知识了。<br/>


### B - Toy Storage 同上面一道题
题目链接：<https://vjudge.net/contest/321263#problem/B><br/>
和上面一道题叙述完全一样，只不过输入不再是有顺序的隔板坐标，而是无序的，需要你重新排序，我觉得我的stl用的还是挺巧妙地（哈哈哈哈哈哈哈哈哈哈）。








