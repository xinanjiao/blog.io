---
layout: post
title: 权值线段树
date: 2019-9-06
categories: blog
tags: [算法,权值线段树]
description: 语言
---
### 权值线段树

1. 维护全局的值域信息，每个节点记录的是该值域的值出现的总次数。
2. 使用二分的思想（离散化的时候，需要用到）
3. 支持查询全局K小值，全局rank，前驱，后继等。
4. 单词操作时间复杂度为O(logn)
5. 空间复杂度为O(n)
6. 相对于平衡树的优势：代码简单，速度快
7. 劣势：值域较大时，我们需要离散化，变成离线数据结构（我认为的离线指的是不能更改插入之类的操作，只能进行查询）

附上b战学习视频一个（主要是后面平衡树解法）<https://www.bilibili.com/video/av16552942?from=search&seid=1412104890759122013><br/>

网上说权值线段树和线段树的操作都差不多，我看了一下，确实差不多，区别也在上面，它维护的是出现的次数，这个和线段树的思想确实不一样，线段树在我看来，就是区间动态修改和动态查询，区间满足可加值。权值线段树将会在下一道题中把它的用处展现得淋漓尽致。

### F - Cloud Computing CodeForces - 1070C 
题目链接：<https://vjudge.net/contest/318111#problem/F><br/>
题目大意：有m mm种借用云计算内核的方案，第i种方案li ri ci pi表示，表示这种方案的可借用时间是从第li 到 ri 天，数量有 ci价格 为pi。公司在1 ~ n 天内如果能租到k件及以上，选择最小价格的k件，否则全部租下，问最后总的价格

思路：由于选最小价值的k件物品，考虑用权值线段树来维护，每天的更新直接用差分来更新，第li天插入，ri+1天减去即可，根据权值线段树的二分性质，权值从小到大的建树方式，所以递归寻找最左边的k件即可，如果小于k件，就全部累加。主要思想是维护在总天数中第i个方案的数量，也就是次数的维护，另外还维护了花费总和这个树，便于以后查找比较（始终注意，树值是总天数，这时与线段树的区别）。


code:

    #define ls l,mid,rt<<1
    #define rs mid+1,r,rt<<1|1
    #define fi first
    #define se second
    typedef pair<ll,ll> P;
    ll gcd(ll a,ll b){return b==0?a:gcd(b,a%b);}
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int INF=0x3f3f3f3f;
    const int maxn=1e6+10;
    //int tree[maxn<<2];
    ll num[maxn<<2];
    ll sum[maxn<<2];
    ll n,k,m;
    vector<P> s[maxn];
    void push(int rt)
    {
       sum[rt]=sum[rt<<1]+sum[rt<<1|1];
       num[rt]=num[rt<<1]+num[rt<<1|1];
    }
    void bulidtree(int l,int r,int rt)
    {
    if(l==r)
    {
        sum[rt]=0;
        num[rt]=0;
        return ;
    }
    int mid=(r+l)>>1;
    bulidtree(ls);
    bulidtree(rs);
    push(rt);
    }
    void updata(ll c,ll p,int l,int r,int rt)
    {
    if(l==r)
    {
        num[rt]+=c;
        sum[rt]+=(p*c);
        return;
    }
    int mid=(l+r)>>1;
    if(p<=mid)
        updata(c,p,ls);
    else
        updata(c,p,rs);
    push(rt);
    }
    ll query(ll x,int l,int r,int rt)
    {
    if(l==r)
    {
        return min(num[rt],x)*l;
    }
    int mid=(r+l)>>1;
    if(num[rt<<1]>=x)
        return query(x,ls);
    else
        return sum[rt<<1]+query(x-num[rt<<1],rs);
    }
    int main()
    {
    ios::sync_with_stdio(false);
    cin>>n>>k>>m;
    fro(i,0,m)
    {
        ll l,r,c,p;
        cin>>l>>r>>c>>p;
        s[l].push_back(P(c,p));
        s[r+1].push_back(P(-c,p));
    }
    ll ans=0;
    // bulidtree(1,maxn,1);
    fro(i,1,n+1)
    {
        fro(j,0,s[i].size())
        {
            updata(s[i][j].fi,s[i][j].se,1,maxn,1);
        }
        ans+=query(k,1,maxn,1);
    }
    cout<<ans<<endl;
     return 0;
    }







