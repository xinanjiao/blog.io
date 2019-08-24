---
layout: post
title: ACM训练第21天（KM二分图最优匹配）
date: 2019-8-19
categories: blog
tags: [算法,图论,数据结构,训练]
description: 语言
---

### 写在前面
图论博大精深，经历一道题一个算法的洗礼。虽然确实痛苦，但是理解算法敲出代码，ac题之后也是很快乐的，ok man 冲冲冲！！！

### 今日份get--带权二分图最优匹配
大佬带你了解二分图：<https://blog.csdn.net/qq_25379821/article/details/83750678><br/>
二分图匈牙利算法:<https://blog.csdn.net/sixdaycoder/article/details/47720471><br/>

## 题目
今天做的题目中有传递闭包（Floyd-Warshall算法实现），二分图最优(最大）匹配，最小生成树+并查集。

### Cow Contest POJ - 3660 传递闭包
题目链接：<https://vjudge.net/problem/POJ-3660><br/>
题目大意：有N头牛，评以N个等级，各不相同，先给出部分牛的等级的高低关系，问最多能确定多少头牛的等级<br/>
解题思路：一头牛的等级，当且仅当它与其它N-1头牛的关系确定时确定，于是我们可以将牛的等级关系看做一张图，然后进行适当的松弛操作，得到任意两点的关系，再对没一头牛进行检查即可。<br/>
此题为传递闭包的实现,如果一个点对n-1个点有关系，那么他就是传递的<br/>

    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int test[101][101];
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    cin>>m>>n;
    fro(i,0,n)
    {
        int a,b;
        cin>>a>>b;
        test[a][b]=1;
    }
    fro(k,1,m+1)
      fro(i,1,m+1)
       fro(j,1,m+1)
       {
           if(test[i][k]&&test[k][j])
            test[i][j]=1;
       }
       int sum=0;
       fro(i,1,m+1)
         {
             int cnt=0;
             fro(j,1,m+1)
             {
                 if(i==j)
                    continue;
                 if(test[i][j]||test[j][i])
                    cnt++;
             }
             if(cnt==m-1)
                sum++;
         }
         cout<<sum<<endl;
    return 0;
    }

### hud 2063 过山车 二分图最大匹配 模板题
题目链接：<https://vjudge.net/problem/HDU-2063><br/>
二分图的最大匹配模板题，涉及匈牙利算法。<br/>

    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int match[501][501];
    int pd[501];
    int book[501];
    int m,n;
    int dfs(int k)
    {
    fro(i,1,n+1)
    {
        if(!book[i]&&match[k][i])
        {
            book[i]=1;
            if(pd[i]==0||dfs(pd[i]))
            {
                pd[i]=k;
               // pd[k]=i;
                return 1;
            }
        }
    }
    return 0;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    while(cin>>t)
    {
        if(t==0)
            break;
        cin>>m>>n;
        mem(match,0);
        mem(pd,0);
        fro(i,0,t)
        {
            int a,b;
            cin>>a>>b;
            match[a][b]=1;//match[b][a]=1;
        }
        int sum=0;
        fro(i,1,m+1)
        {
            mem(book,0);
            if(dfs(i))
                sum++;
        }
        cout<<sum<<endl;
    }
    return 0;
    }

### hdu 2255 奔小康赚大钱 带权二分图最优匹配
题目链接：<https://vjudge.net/problem/HDU-2255><br/>
带权二分图最大匹配模板题。<br/>
值得一提的是，开始我一直时间超限，最后居然是全局变量minsize在主函数重新定义了（醉了醉了。。。)<br/>

### A - 继续畅通工程 HDU - 1879
题目链接：<https://vjudge.net/contest/313341#problem/A><br/>
并查集+最小生成树，关于最小生成树，以后补充，记得找链接看看，算是入门，并查集在这里的用处是看是否有回路。<br/>

### M - Median  传递闭包
题目链接：<https://vjudge.net/contest/320737#problem/M><br/>
求中间的那个数，那个数满足和其他n-1个数都有关系就行了，传递闭包解决。<br/>

### Rank HDU - 1704 传递闭包
题目链接：<https://vjudge.net/problem/HDU-1704><br/>
和其他传递闭包不同的是，只要这个数和其他没有关系，那么就可以计数，求的就是不能确定的数目。<br/>











