---
layout: post
title: ACM训练第二天
date: 2019-7-24
categories: blog
tags: [算法,广度搜索,训练]
description: 语言
---
### Property Distribution (AOJ 0118)
题目链接:<https://vjudge.net/problem/Aizu-0118#author=0><br>
有四种符号组成的矩阵，判断有多少四联通块,染色+dfs<br>
未知错误代码无法显示！！！<br/>

### ball (AOJ 0033)
题目链接:<https://vjudge.net/problem/Aizu-0033#author=0><br/>
**题目大意**<br/>
从1到10连续编号的小球从容器开口A放入。通过调整枢轴E的方向，可以使小球经过D而落入左侧B筒或者右侧C筒。现给出从A放入小球的顺序，请你判断能否最终小球落入B和C时，号码大的球总是位于号码小的球的上侧。如果可能则在一行中输出”YES”，否则输出 “NO”<br/>
<br/>
DFS巧用，实际上不用把落下的小球存起来，只要判断它是否大于前面一个数即可，原来我想多了，不用回溯，不行就是不行，不用考虑其他情况<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <algorithm>
    #include <cmath>
    #include <deque>
    #include <vector>
    #include <queue>
    #include <string>
    #include <cstring>
    #include <map>
    #include <stack>
    #include <set>
    #include <sstream>
    using namespace std;
    const int inf=0x3f3f3f;
    const int maxn=1010;
    int ball[maxn];
    bool ok;
    void dfs(int x,int r,int l)
    {
    if(x==10)
        return;
    if(ball[x]>r)
        dfs(x+1,ball[x],l);
    else if(ball[x]>l)
        dfs(x+1,r,ball[x]);
    else
    {
        ok=true;
        return;
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    {
        for(int i=0;i<10;i++)
            cin>>ball[i];
        ok=false;
        dfs(0,0,0);
        if(!ok)
            cout<<"YES"<<endl;
        else
            cout<<"NO"<<endl;
    }
    }
通过判断前面一个小球的大小，实现DFS的巧用<br/>






