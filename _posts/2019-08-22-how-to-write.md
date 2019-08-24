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
<p style="color: red;">注意：我在做这道题的时候，输出出现了-0,这种情况我以前也遇到过，只不过都是迷迷糊糊就过去了，网上解释说：由于浮点数误差，所以要在后面加一个极小的数，比如此题的EPS</p>


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
解题思路：如果有存在这样的直线，过投影相交区域作直线的垂线，该垂线必定与每条线段相交，问题转化为问是否存在一条线和所有线段相交。

显然所求线段若存在，那么一定可以通过移动使其卡到2个端点上。

所以，枚举所有端点，判断该直线是否合法。

坑点：

1.同一条线段的2个端也需要枚举,因为可能所有线段共线

2.判断所枚举的直线是否退化为点。

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
    int n;
    struct point
    {
     double x,y;
    };
    struct line
    {
    point a,b;
    }line[110];
    double dispoint(point a,point b)
    {
    return sqrt((a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y));
    }
    double chaji(point p1,point p2,point p0)
    {
    return (p2.x-p1.x)*(p0.y-p1.y)-(p0.x-p1.x)*(p2.y-p1.y);
    }
    bool judge(point a,point b)
    {
    if(dispoint(a,b)<EPS)
        return false;
    fro(i,0,n)
    {
        if(chaji(a,b,line[i].a)*chaji(a,b,line[i].b)>EPS)
            return false;
    }
    return true;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        mem(line,0);
        cin>>n;
        fro(i,0,n)
        {
            cin>>line[i].a.x>>line[i].a.y>>line[i].b.x>>line[i].b.y;
        }
        if(n==1)
            puts("Yes!");
        else
        {
        //枚举线段端点
         bool ok=true;
        fro(i,0,n)
        {
             ok=true;
            fro(j,0,n)
            {
                if(judge(line[i].a,line[j].a)||judge(line[i].a,line[j].b)||judge(line[i].b,line[j].a)||judge(line[i].b,line[j].b))
                 {
                    ok=false;
                    break;
                }
            }
            if(!ok)
                break;
        }
        if(!ok)
            puts("Yes!");
        else
            puts("No!");
        }
    }
     return 0;
    }

## 题目
以下就是中国剩余定理的啦，拓中我还不熟悉，以后补充

### poj 2891 奇怪的整数 拓展中国剩余定理 
题目链接：<https://vjudge.net/contest/319143#problem/B><br/>
他来了，他来了，拓中他来了，理解一下上面的链接你就知道拓展中国剩余定理的解法，他是一个迭代的过程，因为不能保证是互质的，所以最后要求解他的最大公约数，每次把解都要更新，求解k个解数的时候，解要是前面k-1的解，最小公约数也是要继续更新<br/>
<p style="color: red;">这种题因为一般是模板，所以会在数据方面卡人。比如数据在long long int 范围，这就要小心了，因为在某些操作，比如乘法中会溢出，所以要不断取模，在乘法中，可以使用快速幂乘（龟速乘），即保证了速度，也可以边算边取模。</p>

    using namespace std;
    #define ll long long  int
    #define fro(i,a,n) for(int i=a;i<n;i++)
    #define pre(i,a,n) for(int i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    ll gcd(ll a,ll b) {return b?gcd(b,a%b):a;}
    const double PI = 3.1415926535897932;
    const double EPS=1e-8;
    const int maxn = 1e5+10;
    const int INF=0x3f3f3f3f;
    ll x,y,k;
    ll a[maxn],r[maxn],ans;
    ll exgcd(ll a,ll b,ll &x,ll &y)
    {
    if(b==0)
    {
        x=1;
        y=0;
        return a;
    }
    ll r=exgcd(b,a%b,x,y);
    ll temp=x;
    x=y;
    y=temp-a/b*y;
    return r;
    }
     ll ksm(ll a,ll b,ll mod)//快速乘
    {
    ll ans=0;
    while(b>0)
    {
     if(b&1)
    {
        ans=(ans+a)%mod;
    }
    a=(a+a)%mod;
    b>>=1;
    }
    // cout<<ans<<endl;
    return ans;
    }
     ll china()
    {
    ll m=a[1],bg;
    ans=r[1];
    fro(i,2,k+1)
    {
        ll aa=a[i],bb=m,c=((r[i]-ans)%aa+aa)%aa;
        ll d=exgcd(bb,aa,x,y);
        if(c%d!=0)
            return -1;
        bg=aa/d;
        x=ksm(x,c/d,bg);
        ans+=x*m;//更新前面方程的解
        m*=bg;//m为K个m的lcm
        ans=(ans%m+m)%m;
        //cout<<ans<<endl;
    }
    return (ans%m+m)%m;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    while(cin>>k)
    {
        mem(a,0);mem(r,0);
        fro(i,1,k+1)
    {
        cin>>a[i]>>r[i];
    }
    cout<<china()<<endl;
    }
     return 0;
    }

### P3868 [TJOI2009]猜数字
题目链接：<https://www.luogu.org/problem/P3868><br/>
中国剩余定理模板题，注意最后一个样例会爆long long int ,所以在乘法计算的时候要取模，这里的乘法计算用了快速幂思想，在乘的过程中不断取模，网上谓之（龟速幂）。

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
    ll x,y;
    ll a[20],b[20];
      ll exgcd(ll a,ll b,ll &x,ll &y)
    {
    if(b==0)
    {
        x=1;
        y=0;
        return a;
    }
    ll r=exgcd(b,a%b,x,y);
    ll temp=x;
    x=y;
    y=temp-a/b*y;
    return r;
    }
     ll ksm(ll a,ll b,ll mod)
    {
    ll ans=0;
    while(b>0)
    {
     if(b&1)
    {
        ans=(ans+a)%mod;
    }
    a=(a+a)%mod;
    b>>=1;
    }
    // cout<<ans<<endl;
    return ans;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int k;
    cin>>k;
    fro(i,0,k)
    {
        cin>>a[i];
    }
    ll sum=1,cnt=0;
    fro(i,0,k)
    {
        cin>>b[i];
        sum*=b[i];
    }
    fro(i,0,k)
    {
        ll lcm=sum/b[i];
        exgcd(lcm,b[i],x,y);
        x=(x%b[i]+b[i])%b[i];
        a[i]=(a[i]%b[i]+b[i])%b[i];
        cnt=(cnt+ksm(ksm(lcm,x,sum),a[i],sum))%sum;
    }
    // cout<<cnt<<endl;
    //cnt=cnt%sum;
    cout<<cnt<<endl;
     return 0;
    }

### 洛谷P4777 【模板】扩展中国剩余定理（EXCRT）
题目链接：<https://www.luogu.org/problem/P4777><br/>



感谢小牛同学的开导，生活依然很美好，因为你。




