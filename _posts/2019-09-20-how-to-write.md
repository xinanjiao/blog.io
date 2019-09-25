---
layout: post
title: 计算几何+素数筛
date: 2019-9-20
categories: blog
tags: [算法,计算几何,素数筛]
description: 语言。
---

### C - Difference Between Primes HDU - 4715 
题目链接:<https://vjudge.net/contest/327245#problem/C><br/>
题目大意：给出一个偶数，要求输出能相加等于这个偶数的两个最小素数。<br/>
思路：<br/>
素数筛显然很好使，但是刚开始我是筛选出素数后，我打算把所有素数的相加可能结果都表示出来（太信任预处理了），最后果不其然t了，实际是找出所有素数后，循环找即可。<br/>

    bool b[maxn];
    int main()
    {
    //ios::sync_with_stdio(false);
    mem(b,true);b[0]=0;b[1]=0;
    for(int i=2;i<=sqrt(maxn);i++)//升级素数筛，复杂度 logn
    {
        if(b[i])
        {
            for(int j=i*i;j<=maxn;j+=i)
            {
                b[j]=false;
            }
        }
    }
    int t;
    s_d(t);
    while(t--)
    {
        int m;
        s_d(m);
        if(m==0)
            cout<<"2 2"<<endl;
        else
        {
            bool ok=0;
        fro(i,3,maxn)
        {
            if(b[i])
            {
            int n=m+i;
            if(n>0)
            {
                if(b[n]==1)
                {
                    printf("%d %lld\n",n,i);
                    ok=1;
                    break;
                }
            }
            }
        }
        if(!ok)
       printf("FAIL\n");
        }
    }
    return 0;
    }












