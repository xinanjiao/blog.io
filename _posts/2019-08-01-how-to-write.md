---
layout: post
title: ACM训练第10天
date: 2019-8-01
categories: blog
tags: [算法,二分+贪心,训练]
description: 语言
---

### 小事项 greedy
今天做的很多贪心题，都差不多贪心与优先队列结合起来了,顺势复习了一波优先队列。<br/>
优先队列与队列不同的是，它可以按照优先级的不同，弹出元素，不是先进先出，所以访问元素也不是front(),而是top(),另外优先顺序需要用结构体来声明，如 <br/>

    struct cmp
    {
       bool operator()(const int a,const int b)const 
       {
       	return a>b;
       }
    };
    priority_queue<ll,vector<ll>,cmp> s;

需要注意的是，排序的cmp，如上代码，与sort()不同的是，如果sort按照这个排法，是由大到小，而优先队列是先弹出小的，这个要注意，而且重载的是括号，不是小于符号，需要在结构体中重载，set也是这样（亲试有效)。<br/>

贪心加二分，二分不难，但贪心策略要选好，所以慢慢总结叭！<br/>


## 题目（代码后补）

### exam (Codeforce 732D)
题目链接：<https://vjudge.net/problem/CodeForces-732D><br/>
网上题解：<https://blog.csdn.net/haut_ykc/article/details/52845645><br/>
这道题做了半天，现在还处于死模拟的阶段，没有想出一个好的贪心策略，网上题解讲的挺好的，好好研究研究，这题很有意义，贪心思想很明显<br/>

code :

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e5+10;
    const int INF=0x3f3f3f3f;
    int n, m;
    int a[100005], b[100005], flag[100005];
    bool cheack(int d)
    {
    mem(flag,false);
    int sum=0;
     for(int i=d;i>=1;i--)
    {
        if(a[i]&&!flag[a[i]])
        {
            flag[a[i]]=1;
            sum+=b[a[i]];
        }
        else if(sum!=0)
            sum--;
    }
    fro(i,1,n+1)
    {
        if(flag[i]==0)
            return false;
    }
    if(sum!=0)
        return false;
    return true;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    cin>>m>>n;
    fro(i,1,m+1)
    {
        cin>>a[i];
    }
    fro(i,1,n+1)
    {
        cin>>b[i];
    }
    int lefta=1;
    int righta=m;
    while(lefta<righta)
    {
        int mid=(righta+lefta)/2;
        if(cheack(mid))
            righta=mid;
        else
            lefta=mid+1;
    }
    if(cheack(lefta))
        cout<<lefta<<endl;
    else if(cheack(righta))
        cout<<righta<<endl;
    else
        cout<<"-1"<<endl;
    }

### B - Fence Repair 
题目链接：<https://vjudge.net/contest/315591#problem/B><br/>
网上题解：<https://blog.csdn.net/sinat_19628145/article/details/51105650><br/>
huffuman原理，哈夫曼树是构造最优二叉树的方法，在离散数学中有所提及，这道题就用了这个思想，每次选最小的两个，怎么实现，就用优先队列<br/>

code :

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e4+10;
    const int INF=0x3f3f3f3f;
    struct cmp
    {
    bool operator()(const ll a,const ll b)const
    {
        return a>b;
    }
    };//排序与sort规则相反，注意是重载括号运算符
    int main()
    {
    priority_queue<ll,vector<ll>,cmp> s;//优先队列
    int m;
    cin>>m;
    fro(i,0,m)
    {
       int a;
       cin>>a;
       s.push(a);
       //ss.push_back(a);
    }
    ll sum=0;
    while(s.size()>1)
    {
       int a1=s.top();
       s.pop();
       int a2=s.top();
       s.pop();
       int a3=a1+a2;
       s.push(a3);
       sum+=a3;
    }
    cout<<sum<<endl;
    }

### F - 湫湫系列故事——消灭兔子 
题目链接：<https://vjudge.net/contest/315591#problem/F><br/>
网上题解<https://blog.csdn.net/amazing_123/article/details/81196784><br/>
优先队列+贪心，排好兔子血量，箭的威力，从大到小，一个个遍历兔子，一旦有箭的威力大于兔子的血量，加入优先队列，每遍历一次，出一次队列（巧妙之处）<br/>

code 

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e5+10;
    const int INF=0x3f3f3f3f;
    int rabbit[maxn];
    struct row
    {
    int value;
    int power;
    bool operator<(const row &a)const
    {
        if(power>a.power)
            return true;
        else
            return false;
    }
    }a[maxn];
    struct cmp
    {
    bool operator()(const row a,const row b)const
    {
        return a.value>b.value;
    }
    };
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    while(cin>>m>>n)
    {
       fro(i,0,m)
       {
           cin>>rabbit[i];
       }
       fro(i,0,n)
       {
           cin>>a[i].power;
       }
       fro(i,0,n)
       cin>>a[i].value;
       sort(rabbit,rabbit+m,greater<int>());
       sort(a,a+n);
       priority_queue<row,vector<row>,cmp > s;
       int temp=0;
       int cur=0;
       ll ans=0;
       bool ok=false;
       fro(i,0,m)
       {
           while(temp<n&&a[temp].power>=rabbit[i])
           {
               s.push(a[temp]);
               temp++;
           }
           if(s.empty())
           {
               ok=true;
               break;
           }
           row money=s.top();
           s.pop();
           ans+=money.value;
       }
       if(ok)
        cout<<"No"<<endl;
       else
       cout<<ans<<endl;
    }
    }

### Commando War
题目链接:<https://vjudge.net/contest/315591#problem/D><br/>
求最小花费的时间，不用去模拟每次时间的减少，直接贪心最大，一个个比较<br/>
原来想的和复杂，考虑到时间重叠，模拟很麻烦，实际上直接每次去枚举一个区间，每次都选最小的区间就行<br/>

code 

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=5e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    int n;
    int case1=1;
    while(cin>>n)
    {
        if(n==0)
            break;
        P a[1001];
        //int sumt=0;
        fro(i,0,n)
        {
            cin>>a[i].second>>a[i].first;
             //sumt+=a[i].second;
        }
        sort(a,a+n,greater<P>());
        int ans=0;
        int temp=0;
        fro(i,0,n)
        {
            temp+=a[i].second;
            ans=max(ans,temp+a[i].first);//逐个比较
        }
        cout<<"Case "<<case1++<<": "<<ans<<endl;
     }
    }


### 贪心+二分的体会

链接：<https://blog.csdn.net/yuzhong9208/article/details/78137433><br/>



