---
layout: post
title: ACM训练第13天
date: 2019-8-11
categories: blog
tags: [算法,数据结构,数学,训练,线段树]
description: 语言
---
## 题目（代码后补）

## 线段树

### I Hate It （线段树区间查询+区间修改）
题目链接：<https://vjudge.net/contest/318111#problem/B><br/>
符合区间加法，最大值=max（左区间最大值，右区间最大值）<br/>
题目大意：很多学校流行一种比较的习惯。老师们很喜欢询问，从某某到某某当中，分数最高的是多少。 
这让很多学生很反感。 

不管你喜不喜欢，现在需要你做的是，就是按照老师的要求，写一个程序，模拟老师的询问。当然，老师有时候需要更新某位同学的成绩。<br/>
<p style="color: red;">线段树用到的地方只要符合区间加法就行，什么叫符合区间加法，像这道题，所有的最大值，就是左边最大值与右边最大值的最大值，最小值如此，加法和也如此。</p>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int a[maxn];
     int tree[maxn*4];
    void bulidtree(int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a[l];
        return;
    }
    int mid=(r+l)/2;
    bulidtree(l,mid,rt*2);
    bulidtree(mid+1,r,rt*2+1);
    tree[rt]=max(tree[rt*2],tree[rt*2+1]);
    }
    void change(int pos ,int a,int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a;//max(a,tree[rt]);
        return;
    }
    int mid=(r+l)/2;
    if(pos<=mid)
        change(pos,a,l,mid,rt*2);
    else
        change(pos,a,mid+1,r,rt*2+1);
    tree[rt]=max(tree[rt*2],tree[rt*2+1]);
    }
    ll query(int al,int ar,int l,int r,int rt)
    {
    if(al<=l&&ar>=r)
    {
        return tree[rt];
    }
    int mid=(r+l)/2;
    ll maxl=-1,maxr=-1;
    if(al<=mid)
        maxl=max(query(al,ar,l,mid,rt*2),maxl);
    if(ar>mid)
        maxr=max(query(al,ar,mid+1,r,rt*2+1),maxr);
    return max(maxr,maxl);
    }
    int main()
    {
    int m,n;
    while(scanf("%d%d",&m,&n)!=EOF)
    {
        fro(i,1,m+1)
        {
            scanf("%d",&a[i]);
        }
        bulidtree(1,m,1);
       char c;
       int a,b;
       fro(i,0,n)
       {
           getchar();
           scanf("%c %d %d",&c,&a,&b);
           if(c=='U')
           {
               change(a,b,1,m,1);
           }
           else if(c=='Q')
           {
               printf("%lld\n",query(a,b,1,m,1));
           }
       }
    }
    }

### Improving the GPA (暴力枚举)
题目链接：<https://vjudge.net/contest/313310#problem/A><br/>
枚举出所有总和的可能<br/>
给出两个数m,n，代表在n个数的平均数是m，在表的信息下，叫你求出最大和最小的绩点。<br/>
我做的时候没有好的思路，结束后看了下网上的题解，由于是100的范围，可以暴力枚举即可。<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    int t;
    cin>>t;
    while(t--)
    {
        int score,n;
        cin>>score>>n;
        double maxs=-INF;
        double mins=INF;
        int sum=score*n;
        int i1,i2,j1,j2,k1,k2,l1,l2;
        fro(i,0,n+1)
        {
          i1=i*100;
          i2=i*85;
          fro(j,0,n-i+1)
          {
              j1=j*84;
              j2=j*80;
              fro(k,0,n-j-i+1)
              {
                  k1=k*79;
                  k2=k*75;
                  fro(l,0,n-j-i-k+1)
                  {
                      l1=l*74;
                      l2=l*70;
                     int maxss=i1+j1+k1+l1+(n-i-k-l-j)*69;
                     int minss=i2+j2+k2+l2+(n-i-k-l-j)*60;
                      if(maxss>=sum&&minss<=sum)
                      {
                          double ss=i*4.0+j*3.5+k*3.0+l*2.5+(n-i-k-l-j)*2.0;
                          double s2=i*4.0+j*3.5+k*3.0+l*2.5+(n-i-k-j-l)*2.0;
                          maxs=max(maxs,ss);
                          mins=min(mins,s2);
                      }
                  }
              }
          }
        }
        printf("%.4lf %.4lf\n",(double)mins/n,(double)maxs/n);
    }
    }

### Just a Joke (积分数学)HDU - 4969 
题目链接：<https://vjudge.net/problem/HDU-4969><br/>
公式推导链接:<https://blog.csdn.net/morejarphone/article/details/51766422><br/>
积分物理公式推导（不会---）<br/>
题目大意：题意：一个人做匀速圆周运动，速度是v1，半径是R，一个人从圆心出发追赶，速度是v2，必须保证每一时刻两个人所在位置和圆心共线。给出追赶者最多能够跑动的距离，求能不能追上。<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int main()
    {
    int n;
    cin>>n;
    while(n--)
    {
        double v1,v2,r,d;
        cin>>v1>>v2>>r>>d;
        double t=r/v1*asin(v1/v2);
        double dd=(v2*t*1.0);
        if(dd>d)
            cout<<"Why give up treatment"<<endl;
        else
            cout<<"Wake up to code"<<endl;
    }
    }

### CS Course (线段树区间修改+区间取值) HDU - 6186
题目链接：<https://vjudge.net/contest/313323#problem/B><br/>
位运算拓展，注意tree数组用结构体要存三个数(尽量不用cin cout)<br/>
奇妙的差分思想:<https://blog.csdn.net/i1020/article/details/77803985><br/>
题目大意：给出一个序列，求删去某个下标的数后，求所有数的& | ^ 运算结果。<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    //int a[maxn];
    struct node
    {
    int r,l;
    int v1,v2,v3;
    }tree[maxn*4];
    int a1,a2,a3;
    void pushup(int rt)
    {
    tree[rt].v1=tree[rt*2].v1&tree[rt*2+1].v1;
    tree[rt].v2=tree[rt*2].v2|tree[rt*2+1].v2;
    tree[rt].v3=tree[rt*2].v3^tree[rt*2+1].v3;
    }
    void bulidtree(int l,int r,int rt)
    {
    tree[rt].l=l;tree[rt].r=r;
    if(l==r)
    {
        int l;
        scanf("%d",&l);
        tree[rt].v1=l;
        tree[rt].v2=tree[rt].v1;
        tree[rt].v3=tree[rt].v1;
        return;
    }
    int mid=(tree[rt].l+tree[rt].r)/2;
    bulidtree(l,mid,rt*2);
    bulidtree(mid+1,r,rt*2+1);
    pushup(rt);
    }
    void query(int al,int ar,int rt)
    {
    if(al==tree[rt].l&&ar==tree[rt].r)
    {
        a1=tree[rt].v1;
        a2=tree[rt].v2;
        a3=tree[rt].v3;
        return ;
    }
    int mid=(tree[rt].l+tree[rt].r)/2;
    if(ar<=mid)
    query(al,ar,rt*2);
     else if(al>mid)
    query(al,ar,rt*2+1);
    else
    {
        int b1,b2,b3;
        query(al,mid,rt*2);
        b1=a1;b2=a2;b3=a3;
        query(mid+1,ar,rt*2+1);
        a1&=b1;a2|=b2;a3^=b3;
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n,q;
    while(scanf("%d%d",&n,&q)!=EOF)
    {
        /*fro(i,1,n+1)
        {
            scanf("%d",&a[i]);
        }*/
        int b1,b2,b3;
        bulidtree(1,n,1);
        fro(i,0,q)
        {
            int x;
            scanf("%d",&x);
            if(x==1||x==n)
            {
                if(x==1)
                {
                    query(2,n,1);
                    //cout<<a1<<" "<<a2<<" "<<a3<<endl;
                }
                else
                {
                    query(1,n-1,1);
                   // cout<<a1<<" "<<a2<<" "<<a3<<endl;
                }
                printf("%d %d %d\n",a1,a2,a3);
                continue;
            }
                query(1,x-1,1);
                b1=a1;b2=a2;b3=a3;
                query(x+1,n,1);
                //cout<<(int)(b1&a1)<<" "<<(int)(b2|a2)<<" "<<(int)(b3^a3)<<endl;
                printf("%d %d %d\n",b1&a1,b2|a2,b3^a3);
        }
    }
    }


### Can you answer these queries? 线段树 区间相加
题目链接:<https://vjudge.net/contest/313335#problem/A><br/>
线段树模板，注意复杂度的降低，开根号最多开7次就会变为一<br/>
题目大意：每次更改区间里面所有值，变为原来的根号，然后查询区间和。<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    ll a[maxn];
    ll tree[maxn*4];
    void bulidtree(int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a[l];
        return;
    }
    int mid=(r+l)>>1;
    bulidtree(l,mid,rt<<1);
    bulidtree(mid+1,r,rt<<1|1);
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    void change(int al,int ar,int l,int r,int rt)
    {
    if(al>r||ar<l)
        return ;
    if(l>=al&&r<=ar&&tree[rt]==(r-l+1))
        return ;
    if(l==r)
    {
        tree[rt]=sqrt(tree[rt]*1.0);
        return;
    }
    int mid=(r+l)>>1;
    if(al<=mid)
        change(al,ar,l,mid,rt<<1);
    if(ar>mid)
        change(al,ar,mid+1,r,rt<<1|1);
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    ll query(int al,int ar,int l,int r,int rt)
    {
    if(al>r||ar<l)
        return 0;
    if(al<=l&&ar>=r)
    {
        return tree[rt];
    }
    int mid=(r+l)>>1;
    ll sum=0;
    if(al<=mid)
        sum+=query(al,ar,l,mid,rt<<1);
    if(ar>mid)
        sum+=query(al,ar,mid+1,r,rt<<1|1);
    return sum;
    }
    int main()
    {
    int n;
    int case1=1;
    while(scanf("%d",&n)!=EOF)
    {
        printf("Case #%d:\n",case1++);
        fro(i,1,n+1)
          scanf("%I64d",&a[i]);
        bulidtree(1,n,1);
          int m;
          scanf("%d",&m);
          fro(i,0,m)
          {
              int q,a,b;
              scanf("%d%d%d",&q,&a,&b);
              if(q==0)
              {
                  if(a>b)
                    swap(a,b);
                  change(a,b,1,n,1);
              }
              else if(q==1)
              {
                  if(a>b)
                    swap(a,b);
                  printf("%I64d\n",query(a,b,1,n,1));
              }
          }
          printf("\n");
    }
    }

### 洛谷线段树  awsl 万恶的TLE
题目链接：<https://www.luogu.org/problem/P3372><br/>
一直TLE，注意懒标记的下放,看样子是数组开小了和数据范围要long long <br/>

    const int maxn=2e6+10;
    const int INF=0x3f3f3f3f;
    int a[maxn];
    ll tree[maxn*4];
    ll Add[maxn*4];//懒惰标记
    void PushDown(ll rt,ll ln,ll rn)//标记下传
    {
	//ln,rn为左子树，右子树的数字数量。
	if(Add[rt])
    {
		//下推标记
		Add[rt<<1]+=Add[rt];
		Add[rt<<1|1]+=Add[rt];
		//修改子节点的Sum使之与对应的Add相对应
		tree[rt<<1]+=Add[rt]*ln;
		tree[rt<<1|1]+=Add[rt]*rn;
		//清除本节点标记
		Add[rt]=0;
	}
    }
    void bulidtree(ll l,ll r,ll rt)
    {
    if(l==r)
    {
        tree[rt]=a[l];
        return;
    }
    ll mid=(r+l)>>1;
    bulidtree(l,mid,rt<<1);
    bulidtree(mid+1,r,rt<<1|1);
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    ll query(ll al,ll ar,ll l,ll r,ll rt)
    {
    if(al<=l&&ar>=r)
    {
        return tree[rt];
    }
    ll mid=(r+l)>>1;
    PushDown(rt,mid-l+1,r-mid);
    ll sum=0;
    if(al<=mid)
        sum+=query(al,ar,l,mid,rt<<1);
    if(ar>mid)
        sum+=query(al,ar,mid+1,r,rt<<1|1);
    return sum;
    }
    void add(ll al,ll ar,ll a,ll l,ll r,ll rt)
    {
    if(al<=l&&ar>=r)
    {
        tree[rt]+=a*(r-l+1);
        Add[rt]+=a;
        return ;
    }
    ll mid=(r+l)>>1;
    PushDown(rt,mid-l+1,r-mid);
    if(al<=mid)
    {
        add(al,ar,a,l,mid,rt<<1);
    }
    if(ar>mid)
    {
        add(al,ar,a,mid+1,r,rt<<1|1);
    }
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    int  main()
    {
    //ios::sync_with_stdio(false);
    int m,n;
    scanf("%d%d",&m,&n);
    //fill(Add,Add+4*maxn,1);
    fro(i,1,m+1)
    {
        scanf("%d",&a[i]);
    }
    bulidtree(1,m,1);
    while(n--)
    {
        int a,b,c,d;
        scanf("%d",&a);
        if(a==1)
        {
            scanf("%d%d%d",&b,&c,&d);
            add(b,c,d,1,m,1);
        }
        else if(a==2)
        {
            scanf("%d%d",&b,&c);
            printf("%lld\n",query(b,c,1,m,1));
        }
    }
    }









