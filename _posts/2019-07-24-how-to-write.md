---
layout: post
title: ACM训练第二天
date: 2019-7-24
categories: blog
tags: [算法,广度搜索,训练]
description: 文章金句。
---
### Property Distribution （AOJ 0118）
题目链接:<https://vjudge.net/problem/Aizu-0118><br>
DFS+染色<br>

    #include <iostream>
    #include <cstdio>
    using namespace std;
    const int inf=0x3f3f3f;
    char map1[110][110];
    int dir[4][2]={{0,1},{0,-1},{1,0},{-1,0}};
    char ch;
    int sum;
    int h,w;
    void dfs(int m,int n,char ch)
    {
    map1[m][n]='.';
    for(int i=0;i<4;i++)
    {
        int hm=m+dir[i][0];
        int hn=n+dir[i][1];
        if(hm>=0&&hm<h&&hn>=0&&hn<w&&map1[hm][hn]==ch)
        {
            map1[hm][hn]='.';
            dfs(hm,hn,ch);
        }
    }
    }
    int main()
    {
    while(cin>>h>>w)
    {
        if(h==0&&w==0)
            break;
            sum=0;
        for(int i=0;i<h;i++)
         for(int j=0;j<w;j++)
         cin>>map1[i][j];
        for(int i=0;i<h;i++)
        {
            for(int j=0;j<w;j++)
            {
                if(map1[i][j]=='*'||map1[i][j]=='#'||map1[i][j]=='@')
                    {
                        ch=map1[i][j];
                        dfs(i,j,ch);
                        sum++;
                    }
            }
        }
        cout<<sum<<endl;
    }
    }










