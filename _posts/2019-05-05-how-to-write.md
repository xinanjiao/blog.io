---
layout: post
title: 正方形
date: 2019-05-06
categories: blog
tags: [算法,uva,枚举]
description: 文章金句。
---

![题目PDF](https://uva.onlinejudge.org/external/2/201.pdf)

### 题目大意

有n行n列的小黑点(2<=n<=0),还有m条线段连接其中的一些黑点，统计形成了几个正方形（每种边长分别统计）。
行从上到下编号为1-n,列也是1-n。具体输入输出看图<br/>

**注意**输入vij的时候行和边是反的。

### 思路
1. 暴力枚举出n行n列所能产生的正方形边数，从1-n进行枚举。
2. 再通过枚举的边长来判断该正方形的几个顶点是否连接。
3. 按格式输出个数。

主要是枚举出边的长度，再通过长度来判断。

    #include<bits/stdc++.h>
    using namespace std;
    int H[10][10];//存横边
    int V[10][10];//存纵边
    int main()
    {
    int i,j,n,m,a,b,flag=0;
    char ch;
    while(cin>>n>>m)
    {
        //getchar();
        memset(H,0,sizeof(H));
        memset(V,0,sizeof(V));
        while(m--)
        {
            cin>>ch>>a>>b;
            if(ch=='H')
                H[a][b]=1;
            if(ch=='V')
                V[b][a]=1;
        }
        if(flag++)cout<<endl<<"**********************************"<<endl<<endl;
        cout<<"Problem #"<<flag<<endl<<endl;
        int sum=0;
        for(int l=1;l<=n;l++)//列举所有边长
        {
            int count1=0,flag=0;
            for(i=1;i+l<=n;i++)
            for(j=1;j+l<=n;j++)
            {
                flag=1;
                for(int h=j;h<j+l;h++)
                if(H[i][h]==0||H[i+l][h]==0)flag=0;
                for(int v=i;v<i+l;v++)
                if(V[v][j]==0||V[v][j+l]==0)flag=0;
                count1+=flag;//查找操作
            }
            sum+=count1;
            if(count1)
                cout<<count1<<" square (s) of size "<<l<<endl;
                else
                    continue;
        }
        if(sum==0)
            cout<<"No completed squares can be found."<<endl;
    }
    }










