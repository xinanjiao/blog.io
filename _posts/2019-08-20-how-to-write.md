---
layout: post
title: ACM训练第22天（最短路径）
date: 2019-8-20
categories: blog
tags: [算法,训练,图论]
description: 语言
---

### 写在前面
进不了B组，我也不怪谁，自己水平问题，怪自己，努力提升自己相比怨天尤人来得快得多。继续加油！

### 今日get---图论最短路径之 Dijkstra算法，Bellmanford算法，Floyd算法
Floyd算法是最好理解，也是最短的算法相比其他两个来说，Floyd算法通过以邻接表的形式储存图，然后分别以三个循环来判断以一个点到另外一个点是否可以间接通过另外一个点缩短距离。核心代码5行。<br/>
dijkstra算法和bellman算法思想差不多，不断实现边的松弛操作，dijkstra算法每次更新从源点到每个点的最短距离，然后每次都通过这个最短距离松弛其他距离。复杂度为这三个当中最低的<br/>
上面两个算法只能用于正权图（FLOyd可以用于负权），而bellman算法可以实现负权中的最短路，而且松弛操作有且仅有顶点个数减一次，如果超过这个次数，就说明有负权环，所以这样也能判断负权环。<br/>
三个算法总结：<https://blog.csdn.net/qq_35644234/article/details/60870719><br/>

## 题目

### 一题多解之 hud 2544 分别用三种算法求解 
【代码在vj中---提醒我自己】<br/>
题目链接：<https://vjudge.net/problem/HDU-2544><br/>
diskstra算法模板：<https://blog.csdn.net/u011721440/article/details/17164891><br/>

分别用以上三种算法均可解决，运行时间Floyd > Bellmanford > diskstra。三种算法均可解决正权图，其中Floyd和bellmandord算法可以解决负权图的最短路径，bellmanford可解决负权环问题。<br/>

法一：folyd算法

    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int map1[101][101];
    bool vis[101][101];
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    while(cin>>m>>n)
    {
       if(!m&&!n)
        break;
       fro(i,1,m+1)
       fro(j,1,m+1)
       {
           if(i!=j)
            map1[i][j]=INF;
           else
            map1[i][j]=0;
       }
       fro(i,0,n)
       {
           int a,b,c;
           cin>>a>>b>>c;
           map1[a][b]=map1[b][a]=c;
       }
       fro(k,1,m+1)
       {
           fro(i,1,m+1)
           {
               fro(j,1,m+1)
               map1[i][j]=min(map1[i][j],map1[i][k]+map1[k][j]);
           }
       }
       cout<<min(map1[1][m],map1[m][1])<<endl;
    }
    return 0;
    }

法二：bellmanford算法(邻接表存图）,注意是无向图，所以要两个方向判断

    struct edge
    {
    int u,v,w;
    }edge[maxn];
    int dis[110];
    int m,n;
    int u[maxn],v[maxn],w[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    while(cin>>m>>n)
    {
        if(!m&&!n)
            break;
           // mem(edge,0);
        fro(i,1,m+1)
          dis[i]=INF;
          dis[1]=0;
          fro(k,1,n+1)
          {
              int a,b,c;
              cin>>a>>b>>c;
              edge[k].u=a;edge[k].v=b;edge[k].w=c;
          }
          fro(j,0,m)
          {
              fro(i,1,n+1)
              {
                  if(dis[edge[i].u]>dis[edge[i].v]+edge[i].w)
                    dis[edge[i].u]=dis[edge[i].v]+edge[i].w;
                    if(dis[edge[i].v]>dis[edge[i].u]+edge[i].w)
                    dis[edge[i].v]=dis[edge[i].u]+edge[i].w;
              }
          }
          cout<<dis[m]<<endl;
    }
    return 0;
    }



### A - Alice's Print Service  c2 预处理+二分
题目链接：<https://vjudge.net/contest/321083#problem/A><br/>
一开始我没有想到后面可以比前面小，这道题主要是预处理，不是二分搜索将会超时！<br/>


### B - Wormholes 最短路之bellman-ford算法 负权+负权回路
题目链接：<https://vjudge.net/contest/320737#problem/B><br/>
bellmanford算法模板：<https://blog.csdn.net/a1097304791/article/details/89433549><br/>
判断负权环可以floyd算法也可以bellmandord算法，其中bellmanford有个升级版算法，也叫做队列优化bellmanford算法（SPFA),它使时间得以优化。后面好好看看，今天没看明白。<br/>








