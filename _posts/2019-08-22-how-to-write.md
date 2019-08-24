---
layout: post
title: ACM训练第24天（中国剩余定理）
date: 2019-8-22
categories: blog
tags: [算法,数学,计算几何]
description: 语言
---
## 写在前面
今天事情发生了挺多的，只不过都不是好事，先是骑车撞石，又是网络赛没有名额了。心情也曾一度失落，这个时候怎样能快速走出低谷？来一发数论练习题吧----一个声音在我脑海中浮现。好！干！一干就一个下午加一个晚上，虽然还不是很清楚，但是终是有收获的，皇天不负有心人，9点的时候ccpc网络赛有名次了哈哈哈，明天好好打比赛，加油加油。

## 中国剩余定理
看了看网上关于这个的很多公式，哇，密密麻麻的，看起来很繁琐，但是我在百度词条看见这是唯一一个被国际认可的中国发明的公式，而且是古人发明的，顿时我心里面油然升起一股敬畏之情（真的），国际上唯一一个中国定理都学不会，还做什么中国人（手动狗头.jpg),所以我就从中午死磕到现在(22:01)，实际上中国剩余定理实现其实并不难，因为我对拓展欧几里得定理有几分熟悉，但是居然还有一个拓展中国剩余定理（这里我以后会说，拓中是解决除数是非互质的，而非拓中是解决除数是互质的）。前者拓展欧几里得解决，后者听说和前面一点关系都没有，居然用的是构造，涉及知识断崖了，所以死磕了很久还是有点不明白，先放一下链接，我会再好好理解理解。<br/>
<p style="color: red;font-size: 30px;">我个人通俗的理解就是：输入除数以及相应的余数，各个除数两两组合，存在他们的公倍数除以其他除数的余数为1，即把那个最小公倍数找到，乘以其他除数对应的余数(找最小公倍数可以以拓殴实现)，以这种形式最后将求解得到最小的一个解，把那个解加上说有除数的最小公倍数的倍数即为所有解。</p>
中国剩余定理：<https://blog.csdn.net/jk_chen_acmer/article/details/81505699><br/>
同上，拓中+剩余定理:<https://blog.csdn.net/niiick/article/details/80229217><br/>

## 计算几何之直线与直线之间的关系（叉积可以实现）
这是一篇关于计算几何中对线段和直线位置关系的介绍:<https://blog.csdn.net/Once_HNU/article/details/6327906><br/>
### 一道题 D - Intersecting Lines 
题目链接：<https://vjudge.net/contest/321263#problem/D><br/>
这道题大意是给出两条线段的端点坐标，叫你判断两个直线的位置关系，有三种，相交（求出坐标），一条直线，平行<br/>叉积在前面用作点与线段之间的位置关系的判断，在这里，我起初是这样想的：利用叉积的几何意义，固定一条直线，判断另外两条端点，如果两个端点都在直线上，那么他们的叉积各为零，如果叉积相乘小于零，则是在两边，大于零在一边。这个思路有个缺点，因为这里是直线，它可以延长，而我这个对线段才有用，所以我一直错。后来把它想为直线就好了，斜率平行则不会相交，叉积各位零则重合，其他就输出交点坐标，推这个交点坐标我写了一页纸（数学不行了）。一切好像万事俱备了。然鹅并没有，我的天！！！！G++中输出浮点数是“f”，和C++不同，C++是“lf"。长知识了。

    using namespace std;
    #define ll long long  int
    #define fro(i,a,n) for(int i=a;i<n;i++)
    #define pre(i,a,n) for(int i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    ll gcd(ll a,ll b) {return b?gcd(b,a%b):a;}
    const double PI = 3.1415926535897932;
    const double EPS=1e-8;
    const int maxn = 3e5+10;
    const int INF=0x3f3f3f3f;
    struct point
    {
     double x,y;
    }a,b,c,d;
    double chaji(point p1,point p2,point p0)
    {
    return (p2.x-p1.x)*(p0.y-p1.y)-(p0.x-p1.x)*(p2.y-p1.y);
    }
    int main()
    {
    //ios::sync_with_stdio(false);
    int t;
    cin>>t;
    puts("INTERSECTING LINES OUTPUT");
    fro(i,0,t)
    {
        cin>>a.x>>a.y>>b.x>>b.y>>c.x>>c.y>>d.x>>d.y;
        if(chaji(a,b,c)==0&&chaji(a,b,d)==0)
        {
            puts("LINE");
        }
        else if((b.x-a.x)*(d.y-c.y)==(d.x-c.x)*(b.y-a.y))
            puts("NONE");
        else //if(chaji(a,b,c)*chaji(a,b,d)<0||(chaji(a,b,c)==0&&chaji(a,b,d)!=0)||(chaji(a,b,d)==0&&chaji(a,b,c)!=0))
        {
            double x,y;
            x=(double)(((c.x*d.y*1.0-d.x*c.y*1.0)*(b.x-a.x)-(a.x*b.y-b.x*a.y)*(d.x-c.x))/((a.y-b.y)*(d.x-c.x)-(c.y-d.y)*(b.x-a.x)))+EPS;
            y=(double)(((a.y-b.y)*(c.x*d.y*1.0-d.x*c.y*1.0)-(c.y-d.y)*(a.x*b.y-b.x*a.y))/((c.y-d.y)*(b.x-a.x)-(a.y-b.y)*(d.x-c.x)))+EPS;
            printf("POINT %.2f %.2f\n",x,y);
        }
    }
    puts("END OF OUTPUT");
     return 0;
    }

### 另外一道题 C - Segments 
题目链接：<https://vjudge.net/contest/321263#problem/C><br/>
问你所以线段是否能投影到一条直线上。<br/>
思路：枚举所有端点，有它组成的直线利用叉积判断与其他端点的关系，必须满足一个在左一个在右，里面有精度控制，如果两个点的距离太近会被认为三点共线。


## 题目
以下就是中国剩余定理的啦，拓中我还不熟悉，以后补充

### poj 2891 奇怪的整数 拓展中国剩余定理 
题目链接：<https://vjudge.net/contest/319143#problem/B><br/>
好吧这是一道拓中题，我以后好好写题解哈。<br/>

### P3868 [TJOI2009]猜数字
题目链接：<https://www.luogu.org/problem/P3868><br/>
中国剩余定理模板题，注意最后一个样例会爆long long int ,所以在乘法计算的时候要取模，这里的乘法计算用了快速幂思想，在乘的过程中不断取模，网上谓之（龟速幂）。


### 洛谷P4777 【模板】扩展中国剩余定理（EXCRT）
题目链接：<https://www.luogu.org/problem/P4777><br/>
好吧由题意，它又是拓中，我要理解它！！！！


感谢小牛同学的开导，生活依然很美好，因为你。




