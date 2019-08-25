---
layout: post
title: ACM训练第14天（并查集）
date: 2019-8-12
categories: blog
tags: [算法,数据结构,训练]
description: 语言。
---

### 今天也是被诅咒的一天啊，TLE它总是围着我，哭了，还好有收获，线段树有很多小细节，一旦出错就会TLE。
### （1）long long int 类型的数据比int 类型的数据更费时。（2）一般看到时间限制在1000ms就不要想线段树了，如果A了，也是运气好，考虑差分数组或树状数组（未学）（3）懒标记下放要注意，区间修改是tage[]=a,区间加上一个数是tage[]+=a,后面的pushdown也有所区别上同。（4）区间修改或值修改注意延迟处理，差不多所有的TLE都是因为它（气死我了！！！）

## 今日get1  并查集
诙谐幽默讲解链接：<https://blog.csdn.net/niushuai666/article/details/6662911><br/>
后面补充

## 今日get2 差分数组
一看就懂系列：<https://blog.csdn.net/zsyz_ZZY/article/details/79918809><br/>
线段树超时就用它，后补。<br/>

## 今日份题目（code后补）

### 畅通工程 并查集
题目链接：<https://vjudge.net/contest/318111#problem/M><br/>
并查集模板，打开了并查集的新世界大门<br/>

    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    int pre[maxn];
    int total;
    int unionfind(int x)//查找函数
    {
    int r=x,temp;
    while(pre[r]!=r)
        r=pre[r];//找到掌门
    while(r!=x)//状态压缩
    {
        temp=pre[x];
        pre[x]=r;
        x=temp;
    }
    return r;
    }
    void join(int x,int y)//合并函数
    {
    int a=unionfind(x);
    int b=unionfind(y);
    if(a!=b)
    {
        pre[a]=b;
        total--;
    }
    }
    int main()
    {
    int m,n;
    while(scanf("%d",&m)&&m)
    {
        total=m-1;
        scanf("%d",&n);
        fro(i,1,m+1)
        {
            pre[i]=i;//自成一派
        }
        fro(i,0,n)
        {
            int a,b;
            scanf("%d%d",&a,&b);
            //开始合并
            join(a,b);
        }
        printf("%d\n",total);
    }
    return 0;
    }

###  Billboard 线段树转换
题目链接：<https://vjudge.net/contest/318111#problem/D><br/>
线段树变形，第一眼我还不知道它是线段树（哭了）<br/>

### 杭电1556 涂气球（Color the ball） 线段树解法 
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=1556><br/>
都说线段树过不了，但是我还是过辽，先一直TLE,过辽因为改了延迟处理

### HDU 1698 Just a Hook 线段树
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=1698><br/>
一样线段树区间修改+查询，一直TLE,原因上同，改了就好了<br/>

### hdu 4970 kill monster 线段树超时 差分数组解法
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=4970><br/>
1000ms限制，线段树我看是没辙了（反正我是试了好久，可能我太弱了），新学差分数组解法，时间复杂度o(n),线段树复杂度o(n* long(n))<br/>


![哪吒](/img/lz.jpg)