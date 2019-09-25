---
layout: post
title: 搜索+线段树（变形）
date: 2019-9-23
categories: blog
tags: [算法,搜索,线段树]
description: 语言
---

### B - Wang Xifeng's Little Plot HDU - 5024（bfs）
题目链接：<https://vjudge.net/contest/328694#problem/B><br/>
题目大意：给出一张图，要求从一个点'.'开始，可以走八个方向，要求只拐一个弯，且等于90度，求能走的最长距离。<br/>
思路：<br/>
初始思路，遍历整张图，找到'.'，分别bfs两次，分别是从上下左右走，和左上左下右上右下，关于90度问题，通过数组的位置取余解决，果不其然T了，我就知道.....，补题，看了题解，题解bfs一次，用数组记录8个方向，且数组中的位置的下标可由取余得到相应的90度的路的步数，整体思路就是把每个点看为拐点，走8个方向。<br/>

    char mas[101][101];
    int dirtion[8][2]={{0,-1},{-1,-1},{-1,0},{-1,1},{0,1},{1,1},{1,0},{1,-1}};
    int dis[10];
    int n;
    int bfs(int a,int b)
    {
    mem(dis,0);
    fro(i,0,8)
    {//将一个点枚举8个方向
        int x=a+dirtion[i][0];
        int y=b+dirtion[i][1];
        while(x>=0&&x<n&&y>=0&&y<n&&mas[x][y]!='#')
        {//一直走
            dis[i]++;
            x+=dirtion[i][0];
            y+=dirtion[i][1];
        }
    }
    int sum=0;
    fro(i,0,8)
    {
        sum=max(sum,dis[i]+dis[(i+2)%8]+1);//点睛之笔，方向安排得很巧妙，通过除余就可以得到
    }//对应的直角方向的值
    return sum;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    while(cin>>n)
    {
       if(n==0)
        break;
        mem(dis,0);
       fro(i,0,n)
       {
           fro(j,0,n)
           cin>>mas[i][j];
       }
       int maxsum=0;
       fro(i,0,n)
         fro(j,0,n)
         {
             if(mas[i][j]=='.')//把每个点当为拐点
             {
                 maxsum=max(maxsum,bfs(i,j));
             }
         }
         cout<<maxsum<<endl;
    }
    }













