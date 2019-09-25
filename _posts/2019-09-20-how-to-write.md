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

### D - Herding HDU - 4709 
题目链接：<https://vjudge.net/contest/327245#problem/D><br/>
题目大意：给出几个坐标，要求计算所有坐标围成最小的面积。如果没有就输出“impoissbale"。<br/>
思路:<br/>
范围100，可三层循环暴力，每次求三个点围成的三角形即可，重点是计算三角形的面积，这里用到了叉积（计算几何），暑假学过，在回忆一下，叉积表示两个向量围成的平行四边形的面积，而一半就是三角形面积，具体看代码。<br/>
<p style="color: red;">重点：当叉积为零时，三点共线</p>

    struct point
    {
    double x,y;
    }a[110];
    double chaji(point a,point b,point c)
    {
    return ((b.x-a.x)*(c.y-a.y)-(c.x-a.x)*(b.y-a.y))/2.0;//两向量的叉积
    }
    int main()
    {
    //ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        int n;
        cin>>n;
        mem(a,0);
        fro(i,0,n)
        cin>>a[i].x>>a[i].y;
        double mina=INF*1.0;
        bool flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=i+1;j<n;j++)
            {
                for(int k=j+1;k<n;k++)
                {
                    double arer=fabs(chaji(a[i],a[j],a[k]));
                    if(arer!=0)
                    {
                        flag=1;
                        mina=min(mina,arer);
                    }
                }
            }
        }
        if(flag)
        printf("%.2lf\n",mina);
        else
            printf("Impossible\n");
    }
    return 0;
    }











