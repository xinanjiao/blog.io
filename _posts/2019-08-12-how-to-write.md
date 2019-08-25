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
线段树超时就用它<br/>
<p style="color: red;">差分数组顾名思义，肯定是数组，什么数组呢，那就是差分，比如先给你一个数组a数组里面的元素都已知，那么我们要用一个b数组来储存他的各项之差，在用一个数组f存储b的各项之差，再用一个数组储存sum储存f的各项之差，最后查询区间a,b的值的话，就直接sum[b]-sum[a],好像差分数组只能求区间之和，也算一个限制</p>

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
线段树变形，第一眼我还不知道它是线段树（哭了），和以前的线段树不一样的是。以前线段树都是查询值，而这道题的线段树是查询点，而且建树方面也有学问，取了一个最小值。查询点和查询值不一样的是查询点是通过tree数组的值不同来更改，而查询点则是通过点来更改。<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn*4];
    int ans,w;
    void bulidtree(int l,int r,int rt)
    {
    tree[rt]=w;
    if(l==r)
    {
        return ;
    }
    int mid=(r+l)/2;
    bulidtree(l,mid,rt<<1);
    bulidtree(mid+1,r,rt<<1|1);
    }
    int query(int a,int l,int r,int rt)
    {//值的查找与单点修改有点相似
    //前者通过值来判断区间，后者通过位置判断
    if(l==r)
    {
        tree[rt]-=a;
        return l;
    }
    int aa=0;
    int mid=(r+l)/2;
    if(tree[rt<<1]>=a)//从左子树开始收索，从上到下
        aa=query(a,l,mid,rt<<1);
    else
        aa=query(a,mid+1,r,rt<<1|1);
    tree[rt]=max(tree[rt<<1],tree[rt<<1|1]);
        return aa;
    }
    int main()
    {
    int h,n;
    while(scanf("%d%d%d",&h,&w,&n)!=EOF)
    {
      h=min(h,n);
        bulidtree(1,h,1);
        fro(i,0,n)
        {
            int s;
            scanf("%d",&s);
            ans=-1;
            if(tree[1]<s)//tree数组第一个位置最大
                ans=-1;
            else
            {
                ans=query(s,1,h,1);
            }
            printf("%d\n",ans);
        }
    }
    }

### 杭电1556 涂气球（Color the ball） 线段树解法 
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=1556><br/>
都说线段树过不了，但是我还是过辽，先一直TLE,过辽因为改了延迟处理。两种解法。<br/>
1.线段树解法，时间1049ms,之前一直超时是在查询的时候，没有做延迟处理，所谓延迟处理下面有注释。

    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int tree[4*maxn];
    int tage[4*maxn];
    void pushdown(int l,int r,int rt)
    {
    if(tage[rt])
    {
        tage[rt<<1]+=tage[rt];
        tage[rt<<1|1]+=tage[rt];
        tree[rt<<1]+=tage[rt]*l;
        tree[rt<<1|1]+=tage[rt]*r;
        tage[rt]=0;
    }
    return ;
    }
     void bulidtree(int l,int r,int rt)
    {
    if(l==r)
        return;
    int mid=(l+r)>>1;
    bulidtree(l,mid,rt<<1);
    bulidtree(mid+1,r,rt<<1|1);
    }
    void change(int al,int ar,int a,int l,int r, int rt)
    {
    if(al<=l&&ar>=r)//罪魁祸首 woc
    {
        tree[rt]+=a*(r-l+1);//减少时间
        tage[rt]+=a;
        return;
    }
    int mid=(l+r)>>1;
    pushdown(mid-l+1,r-mid,rt);
    if(al<=mid)
        change(al,ar,a,l,mid,rt<<1);
    if(ar>mid)
        change(al,ar,a,mid+1,r,rt<<1|1);
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    return;
    }
    int query(int al,int ar,int l ,int r,int rt )
    {//long long 比int更耗时
    if(l==r)
        return tree[rt];
    int mid=(r+l)>>1;
    pushdown(mid-l+1,r-mid,rt);
    int sum=0;
    if(al<=mid)
        sum+=query(al,ar,l,mid,rt<<1);
    if(ar>mid)
        sum+=query(al,ar,mid+1,r,rt<<1|1);
    return sum;
    }
    int main()
    {
    int n;
    while(scanf("%d",&n)!=EOF)
    {
        if(n==0)
            break;
        mem(tree,0);
        mem(tage,0);
        bulidtree(1,n,1);
        fro(i,0,n)
        {
            int a,b;
            scanf("%d%d",&a,&b);
            change(a,b,1,1,n,1);
        }
        fro(i,1,n+1)
        {
            printf(i==1?"%d":" %d",query(i,i,1,n,1));
        }
        printf("\n");
    }
    }

2.差分数组解法

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int a[maxn],b[maxn],c[maxn],sum[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    while(cin>>n&&n)
    {
        mem(a,0);mem(b,0);mem(c,0);mem(sum,0);
        fro(i,0,n)
        {
            int s,m;
            cin>>s>>m;
            b[s]+=1;
            b[m+1]-=1;
        }
        fro(i,1,n+1)
        {
            c[i]=c[i-1]+b[i];
            sum[i]=sum[i-1]+c[i];
        }
        fro(i,1,n+1)
        {
            if(i==1)
            cout<<sum[i]-sum[i-1];
            else
                cout<<" "<<sum[i]-sum[i-1];
        }
        cout<<endl;
    }
    return 0;
    }

### HDU 1698 Just a Hook 线段树
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=1698><br/>
一样线段树区间修改+查询，一直TLE,原因上同，改了就好了<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn*4];
    int add[maxn*4];
    int a[maxn];
    int m;
    void pushdown(int l,int r,int rt)
    {
    if(add[rt])
    {
        add[rt<<1]=add[rt];
        add[rt<<1|1]=add[rt];
        tree[rt<<1]=add[rt]*l;
        tree[rt<<1|1]=add[rt]*r;
        add[rt]=0;
    }
    }
    void bulidtree(int l,int r,int rt)
    {
    if(l==r)
    {
        tree[rt]=a[r];
        return ;
    }
    int mid=(r+l)/2;
    bulidtree(l,mid,rt<<1);
    bulidtree(mid+1,r,rt<<1|1);
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    void change(int al,int ar,int a,int l,int r,int rt)
    {
    if(al<=l&&ar>=r)
    {
      tree[rt]=a*(r-l+1);
      add[rt]=a;
      return ;
    }
    int mid=(r+l)/2;
     pushdown(mid-l+1,r-mid,rt);
    if(al<=mid)
        change(al,ar,a,l,mid,rt<<1);
    if(ar>mid)
        change(al,ar,a,mid+1,r,rt<<1|1);
    tree[rt]=tree[rt<<1]+tree[rt<<1|1];
    }
    int query(int al,int ar,int l,int r,int rt)
    {
    if(al<=l&&ar>=r)
    {
        return tree[rt];
    }
    int sum=0;
    int mid=(r+l)/2;
    pushdown(mid-l+1,r-mid,rt);
    if(al<=mid)
        sum+=query(al,ar,l,mid,rt<<1);
    if(ar>mid)
        sum+=query(al,ar,mid+1,r,rt<<1|1);
    return sum;
    }
    int main()
    {
    int n;
    while(scanf("%d",&n)!=EOF)
    {
        int case1=1;
        while(n--)
    {
        mem(add,0);
        mem(tree,0);
        mem(a,0);
        int aa;
        scanf("%d %d",&m,&aa);
        fro(i,1,m+1)
          a[i]=1;
        bulidtree(1,m,1);
        fro(i,0,aa)
        {
            int s,d,f;
            scanf("%d%d%d",&s,&d,&f);
            change(s,d,f,1,m,1);
        }
    printf("Case %d: The total value of the hook is %d.\n",case1++,tree[1]);
    }
    }
    }

### hdu 4970 kill monster 线段树超时 差分数组解法
题目链接：<http://acm.hdu.edu.cn/showproblem.php?pid=4970><br/>
1000ms限制，线段树我看是没辙了（反正我是试了好久，可能我太弱了），新学差分数组解法，时间复杂度o(n),线段树复杂度o(n* long(n))<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;//差分数组实现
    ll a[maxn],d[maxn],sum[maxn],f[maxn];
    //a原始数组,d差分数组,sum为f前缀和,f为d的前缀和
    int  main()
    {
    int n;
    ios::sync_with_stdio(false);
    while(cin>>n&&n)
    {
        mem(a,0);mem(d,0);mem(sum,0);mem(f,0);
        int m;
        cin>>m;
        fro(i,1,m+1)
        {
            int s1,s2,s3;
            cin>>s1>>s2>>s3;
            a[s1]+=s3;
            a[s2+1]-=s3;
        }
        fro(i,1,n+1)
        {
            f[i]=f[i-1]+a[i];
            sum[i]=sum[i-1]+f[i];
        }
        int sum1=0;
        int ss;
        cin>>ss;
        fro(i,0,ss)
        {
            ll s1,s2;
            cin>>s1>>s2;
            ll a=s1-(sum[n]-sum[s2-1]);
            if(a>0)
                sum1++;
        }
        cout<<sum1<<endl;
    }
    }

![哪吒](/img/lz.jpg)