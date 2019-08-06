---
layout: post
title: ACM训练第9天
date: 2019-7-31
categories: blog
tags: [算法,训练,二分+贪心]
description: 语言
---
## 写在前面
今天B组比赛队友发挥还行，我就划水了，哈哈哈，但后面出了点意外,,,,但今天我们的表现还是可圈可点的，配合也更加成熟了，虽然今天死活没有A出B题，但是我后来看一看里面的贪心+二分这道题，恰好最近的专题训练也是贪心+二分，所以特地学了学，（题做过越多则越熟练！！！)

## 贪心+二分（代码后补）
### Aggressive cows （POJ 2456）
题目链接：<https://vjudge.net/problem/POJ-2456><br/>
题目大意：农夫为了防止牛打架，决定把每头牛分开放在离其他牛尽可能远的位置，求最大的最小距离。类似最大化最小值或者最小值最大化问题，通常用二分搜索就可以很好解决，再加上贪心搜索就可以了<br/>
枚举答案，在贪心寻找，简单区间枚举<br/>

code
 
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=5e5+10;
    const int INF=0x3f3f3f3f;
    int m,n;
    int a[maxn];
    int cheack(int temp)//贪心寻找
    {
    int count1=1;
    int p=a[0];
    fro(i,0,m)
    {
        if(a[i]-p>=temp)
            {
                count1++;
                p=a[i];
            }
    }
    return count1;
    }
    int main()
    {
    cin>>m>>n;
    fro(i,0,m)
    cin>>a[i];
    sort(a,a+m);
    int lefta=0;
    int righta=a[m-1];
    while(lefta<=righta)//区间枚举
    {
        //cout<<1<<endl;
        int mid=(lefta+righta)/2;
        if(cheack(mid)<n)
        {
            righta=mid-1;
        }
        else
            lefta=mid+1;
    }
    cout<<lefta-1<<endl;
    }

### Pie（POJ 3122)
题目链接:<https://vjudge.net/problem/POJ-3122><br/>
把蛋糕最多均分给m+1个人，问最大的分法，但是注意只能同一个蛋糕分，不能不同蛋糕凑成一个体积<br/>
题目大意：我有N个不同口味、不同大小的派。<br/>

有F个朋友会来参加我的派对，每个人会拿到一块派（不能由几个派的小块拼成；可以是一整个派）。<br/>
所有人拿到的派必须是同样大小的（但不需要是同样形状的）。<br/>
当然，我也要给自己留一块，而这一块也要和其他人的同样大小。<br/>
每个派都是一个高为1，半径不等的圆柱体。<br/>
请问我们每个人拿到的派最大是多少？<br/>

题目思路：找到最大的蛋糕体积，开始二分查找答案，因为要么蛋糕不能凑成一个体积，只能一个分，所以贪心时只对每个贪心除就行<br/>

code
   
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-4;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    double r[maxn];
    int m,n;
    bool cheack(double a)
    {
    int temp=0;
    fro(i,0,m)
    {
        temp+=(int)(r[i]/a);
    }
    if(temp<n)
        return true;
    else
        return false;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        cin>>m>>n;
        n+=1;
        fro(i,0,m)
         cin>>r[i];
        double maxd=0.0;
        fro(i,0,m)
        {
            r[i]=PI*r[i]*r[i];
            maxd=max(maxd,r[i]);
        }
        double lefts=0;
        double rights=maxd;
        while(rights-lefts>=EPS)
        {
            double mid=(rights+lefts)/2;
            if(cheack(mid))
                rights=mid;
            else
                lefts=mid;
        }
        printf("%.4lf\n",lefts);
    }
    }


### Cable master(POJ 1064)
题目链接:<https://vjudge.net/problem/POJ-1064><br/>
注意输出时的floor()函数，为向下取整函数，目的时因为题目有限制，一根木棒最多切100次<br/>
贪心加二分<br/>

code

    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    int n,m;
    double a[maxn];
    bool cheack(double s)
    {
    if(s==0)
        return false;
    else
        {
           int temp=0;
    fro(i,0,m)
    {
        temp+=(int)(a[i]/s);
    }
    if(temp<n)
        return true;
    else
        return false;
        }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    cin>>m>>n;
    double maxd=0.0;
    fro(i,0,m)
    {
        cin>>a[i];
        maxd=max(maxd,a[i]);
    }
    double lefta=0.0;
    double righta=maxd;
    while(righta-lefta>=EPS)
    {
        double mid=(righta+lefta)/2;
        if(cheack(mid))
            righta=mid;
        else
            lefta=mid;
    }
    printf("%.2lf\n",floor(righta*100)/100);//floor向下取整
    //ceil()向上取整函数
    }

### K Best （POJ 3111）
题目链接：<https://vjudge.net/problem/POJ-3111><br/>
这道题时均分值问题的升级版，均分问题只叫输出均分的最大数值，而这是问你选哪几个编号，这就要开结构体储存标号<br/>

code

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=5e5+10;
    const int INF=0x3f3f3f3f;
    struct node
    {
    int v,w,id;
    double y;
    }a[maxn];
    int w[maxn],v[maxn];
    double cur[maxn];
    int m,n;
    bool cmp(node a,node b)
    {
    return a.y>b.y;
    }
    bool cheack(double aa)
    {
    fro(i,0,m)
    {
        a[i].y=a[i].v-a[i].w*aa;//因为是平均值，所以看每个
        //乘以平均值后的数值
    }
    sort(a,a+m,cmp);
    double sum=0;
    fro(i,0,n)//贪心选最大
    {
        sum+=a[i].y;
    }
    return sum>=0;//sum大于零说明平均值小了
    }
    int main()
    {
    cin>>m>>n;
    double maxd=0;
    fro(i,0,m)
    {
        cin>>a[i].v>>a[i].w;
        a[i].id=i+1;
        maxd=max(maxd,(double)a[i].v/a[i].w);
    }
    double rightd=maxd;
    double leftd=0;
    while(rightd-leftd>1e-6)//二分
    {
        double mid=(rightd+leftd)/2;
        if(cheack(mid))
            leftd=mid;
        else
            rightd=mid;
    }
    //cout<<leftd<<endl;
    fro(i,0,n)
    {
        if(i==0)
            cout<<a[i].id;
        else
            cout<<" "<<a[i].id;
    }
    cout<<endl;
    }
    
### Strange fuction (HDU 2899)
题目链接：<https://vjudge.net/problem/HDU-2899><br/>
给你一个计算公式，求他当x为1到100时的最大值，由数学公式知道，它是递增函数，就二分1到100，求导，二分+贪心,注意二分跳出条件1e-6<br/>









