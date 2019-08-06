---
layout: post
title: ACM训练第8天
date: 2019-7-30
categories: blog
tags: [算法,训练]
description: 文章金句。
---
### 体会
第一场c1个人赛，又是一场焦灼的3个小时，不是说好难，就A题来说叭。20分钟看懂了题，写好了代码，提交了，不出意外时间超限，我想到了，因为复杂度估计是o(n2+n),我以为这个复杂度就没办法做了，我就放弃了，想了很多什么技巧的方法，可惜，想了一个多小时，完全不行啊，有点凑样例的感觉，后来看了一下题解，完全是我的思路嘛，就优化了一下复杂度，唉，以后不能急着换思路，好好想想能不能去优化。<br>
下午的个人赛C2依然焦灼，今天TM脑子好像就不在线一样，I题看榜2分钟有人A出来了，肯定就不难，我就去看了，真的不难，但是题目我是真没有看懂，我醉了，最后还是问了队友（虽然是个人赛。。。。），啊啊啊，我这英语理解能力是真的不行啊，真得好好练练了，千万不要被题的字数吓到！！！，真的有些时候，暴力不行就想技巧，C2大部分题都是技巧<br/>
C3是贪心和二分，二分得好好学学，贪心简单的简单，也得好好和其它算法结合起来，那就很难了,加油吧<br/>
### 题（代码后补）
1. I - Heist (很简单，但还是长个记性叭)<br/>
题目链接:<https://vjudge.net/contest/313317#problem/I><br/>
题目大意：一个歹徒抢劫，他只会按照编号抢，输入剩下没有被抢的店标号，输出他抢了几个店<br/>

code 
 
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=1e9+10;
    const int INF=0x3f3f3f3f;
    int a[1001];
    int main()
    {
    int n;
    cin>>n;
    fro(i,0,n)
    {
        cin>>a[i];
    }
    sort(a,a+n);
    int sum=a[n-1]-a[0]+1;
    cout<<sum-n<<endl;
    }

2. J - Buying a TV Set （技巧，不要暴力）<br/>
题目链接:<https://vjudge.net/contest/313317#problem/J><br/>
题目大意：输入整数a,b,c,d。c和d是要买电视的尺寸，如c/d，问在a到b的数中满足c/d的尺寸有多少个。此题暴力必超时，技巧就是，a/c和b/d比较当中最小的一个就行！！<br/>

code 
  
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=1e9+10;
    ll gcd(ll a,ll b)
    {
    return b?gcd(b,a%b):a;
    }
    int main()
    {
    ll a,b;
    ll x,y;
    cin>>a>>b>>x>>y;
    ll gg;
    if(x<y)
    {
        gg=gcd(y,x);
    }
    else
        gg=gcd(x,y);
    x/=gg;y/=gg;
    //cout<<x<<" "<<y<<endl;
    ll aa=a/x;
    ll bb=b/y;
    cout<<min(aa,bb)<<endl;
    }

3. K - Medians and Partition（技巧，也是不要模拟！！！)<br/>
题目链接:<https://vjudge.net/contest/313317#problem/K><br/>
题目大意：输入两个整数m,n。m表示数组中有m个数，求该数组能分为几个子数组，该每个子数组满足中位数大于等于n,模拟了半天，最后还是错的，实际上只要找到数组中大于n的个数减去小于n的个数就行了<br/>

code

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    int a[maxn];
    int suba[maxn];
    int m;
    int main()
    {
    int n;
    cin>>n>>m;
        mem(suba,0);
        mem(a,0);
    int counta=0;
    int countb=0;
    int sum=0;
    fro(i,0,n)
    {
        cin>>a[i];
    }
    fro(i,0,n)
    {
        if(a[i]>=m)
            counta++;
        else
            countb++;
    }
    if(counta-countb>=1)
        sum=counta-countb;
    cout<<sum<<endl;
    }

4. H - Theater Square （这道题虽然也是技巧，但读懂题也很难，长一波记性)<br>
题目链接:<https://vjudge.net/contest/313317#problem/H><br/>
5. D - Strange fuction （C3题，二分找答案，范围注意1e-6）<br/>
题目链接:<https://vjudge.net/contest/313332#problem/D><br>
题目大意：Now, here is a fuction: 
  F(x) = 6 * x^7+8*x^6+7*x^3+5*x^2-y*x (0 <= x <=100) 
Can you find the minimum value when x is between 0 and 100.<br/>
贪心+二分模板可套用！！！<br/>

code
  
    typedef pair<int,int> P;
    const double EPS=1e-6;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    double y;
    double reslut(double x,double y)
    {
    return 6*pow(x,7)+8*pow(x,6)+7*pow(x,3)+5*pow(x,2)-y*x;
    }
    double daohan(double x,double y)
    {
    return 42*pow(x,6)+48*pow(x,5)+21*pow(x,2)+10*x-y;
    }
    int main()
    {
    int t;
    cin>>t;
    double mid;
    while(t--)
    {
        cin>>y;
        double lefta=0.0;
        double righta=100.0;
        while(righta-lefta>=EPS)//二分
        {
             mid=(righta+lefta)/2;
            if(daohan(mid,y)>0)
               righta=mid;
            else
               lefta=mid;
        }
        printf("%.4lf\n",reslut(mid,y));
    }
    }













