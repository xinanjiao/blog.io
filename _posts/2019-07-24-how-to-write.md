---
layout: post
title: ACM训练第二天
date: 2019-7-24
categories: blog
tags: [算法,广度搜索,训练]
description: 文章金句。
---


## BFS专练
### Meteor Shower (POJ 3669)
题目链接：<https://vjudge.net/problem/POJ-3669><br/>
**题目大意**<br/>
Bessie听说有场史无前例的流星雨即将来临；有谶言：陨星将落，徒留灰烬。为保生机，她誓将找寻安全之所（永避星坠之地）。目前她正在平面坐标系的原点放牧，打算在群星断其生路前转移至安全地点。
Bessie在0时刻时处于原点，且只能行于第一象限，以平行与坐标轴每秒一个单位长度的速度奔走于未被毁坏的相邻（通常为4）点上。在某点被摧毁的刹那及其往后的时刻，她都无法进入该点。

寻找Bessie到达安全地点所需的最短时间。<br/>
BFS,我当初也一直模拟发现判断太多了，网上给出一种很好的方法，预处理，把流星摧毁的地区标记好，地区的值是流星到达那里的时间，先把它全部枚举出来，后续判断就不会很复杂(orz)。保存状态<br>

    #include <iostream>
    using namespace std;
     #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    const int inf=0x3f3f3f;
    const int maxn=400;
    const int maxr=5e4+10;
    int map1[maxn][maxn];
    int book[maxn][maxn];
    int n;
    int time2=0;
    int dir[5][2]={{1,0},{0,1},{-1,0},{0,-1},{0,0}};
    struct bome
    {
      int x,y;
    int time;
    }bome1[maxr];
    struct girl
    {
    int x;
    int y;
    int time2;
    }a,b,c;
    bool cmp(bome a,bome b)
    {
    return a.time<b.time;
    }
    int bfs()
    {
    a.x=0;a.y=0;
    a.time2=0;
    queue<girl> s;
    s.push(a);
    book[a.x][a.y]=1;
    while(!s.empty())
    {
        b=s.front();
        s.pop();
        fro(i,0,4)//女孩移动
        {
            a.x=b.x+dir[i][0];
            a.y=b.y+dir[i][1];
            a.time2=b.time2+1;
            if(a.x>=0&&a.y>=0&&(map1[a.x][a.y]>a.time2||map1[a.x][a.y]==-1)&&!book[a.x][a.y])
            {
                book[a.x][a.y]=1;
                s.push(a);
                if(map1[a.x][a.y]==-1)
                {
                    return a.time2;
                }
            }
        }
    }
    return -1;
    }
    int main()
    {
    cin>>n;
    fro(i,0,n)
    {
        cin>>bome1[i].x>>bome1[i].y>>bome1[i].time;
    }
    mem(map1,-1);mem(book,0);
    sort(bome1,bome1+n,cmp);
    fro(i,0,n)
        {
        int t=bome1[i].time;
        fro(j,0,5)//四周摧毁
        {
            if((bome1[i].x+dir[j][0]>=0)&&(bome1[i].y+dir[j][1]>=0)&&(t<=map1[bome1[i].x+dir[j][0]][bome1[i].y+dir[j][1]]||map1[bome1[i].x+dir[j][0]][bome1[i].y+dir[j][1]]==-1))
                {
                map1[bome1[i].x+dir[j][0]][bome1[i].y+dir[j][1]]=bome1[i].time;
                }
        }
        }
        cout<<bfs()<<endl;
    }











