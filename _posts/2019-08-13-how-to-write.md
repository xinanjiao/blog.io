---
layout: post
title: ACM训练第15天（字典树）
date: 2019-8-13
categories: blog
tags: [算法,训练,数据结构]
description: 文章金句。
---

### 昨天是TLE的一天，那么今天就是RE的一天，实际上也不是很多RE，只不过做到了一道，巨坑的题吗，light oj上的。RE解决策略：(1)数组访问是否越界。（2）数组是否开小了，有时候开大了也不行（3）输入输出尽量选用较快输入输出........

### 今日份get ---- 字典树
字典树入门精讲：<https://blog.csdn.net/jiutianhe/article/details/8076835><br/>
字典树入门题：<https://blog.csdn.net/qq_38891827/article/details/80532462><br/>

<p style="color: red;">字典树顾名思义就是把字符串像字典一样建成一棵树，树的结点没有字符，字符只是每个结点之间的连接线，字典树的查找和修改都是线性的，我喜欢用数组模拟字典树，root二维数组储存每个结点到下一个结点之间的字符，bool类型的一个数组判断下一个结点是否还有字符连接，可以用于查询操作，coun数组用于记录每个结点的字符的次数，由于查询和修改操作。</p>
## 题目（代码后补）

### 字典树 light oj 1224 DNA Prefix 坑得死人（数组开小了一直re）
题目链接：<https://vjudge.net/problem/LightOJ-1224><br/>
这就是坑人的题了，数组开大了re,开小了re，我也是服了，与也算是一道模板题，但需要注意的是，节约时间要读的时候确定最大值<br/>

    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=2e6+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn][4];
    int coun[maxn];
    bool ok[maxn];
    int maxlen;
    int pc;
    map<char,int> m;
    int change(char a)
    {
    return m[a];
    }
    void bulidtree(char *a)
    {
    int root=0;
    int len=strlen(a);
    for(int i=0;i<len;i++)
    {
        int s=change(a[i]);
        if(!tree[root][s])
            tree[root][s]=++pc;
         coun[tree[root][s]]++;
         maxlen=max(maxlen,coun[tree[root][s]]*(i+1));
         root=tree[root][s];
    }
    ok[root]=true;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    scanf("%d",&t);
    m['A']=0;
    m['C']=1;
    m['G']=2;
    m['T']=3;
    int case1=1;
    while(t--)
    {
    mem(tree,0);
    mem(coun,0);
    mem(ok,false);
    pc=0;
        int a;
        scanf("%d",&a);
        char s[55];
        maxlen=-INF;
        fro(i,0,a)
        {
            scanf("%s",s);
            bulidtree(s);
        }
        //cout<<"Case "<<case1++<<": "<<maxlen<<endl;
        printf("Case %d: %d\n",case1++,maxlen);
    }
    return 0;
    }

### hdu 字典树 1251统计难题  模板题
题目链接：<https://vjudge.net/problem/HDU-1251><br/>
字典树题，结点记录前缀次数<br/>

    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
     typedef pair<int,int> P;
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=2e6+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn][27];
    int coun[maxn];
     int pc;
    bool ok[maxn];
    void bulidtree(char *a)
    {
    int len=strlen(a);
    ll root=0;
    fro(i,0,len)
    {
       int s=a[i]-'a';
       if(!tree[root][s])
        tree[root][s]=++pc;
        root=tree[root][s];
        coun[root]++;
    }
    ok[root]=true;
    }
     ll searchtree(char *a)
    {
    int len=strlen(a);
    ll root=0;
    fro(i,0,len)
    {
        int s=a[i]-'a';
        if(!tree[root][s])
            return 0;
        root=tree[root][s];
    }
    return coun[root];
    }
    int main()
    {
    ios::sync_with_stdio(false);
    char s[11];
    char ser[11];
    mem(ok,false);
    while(gets(s))
    {
        if(s[0]=='\0')break;
      bulidtree(s);
    }
    while(scanf("%s",ser)!=EOF)
    {
      ll pos=searchtree(ser);
        cout<<pos<<endl;
    }
    return 0;
    }

### hdu 字典树 2072单词数  模板题
题目链接:<https://vjudge.net/problem/HDU-2072><br/>
字典树可以很好解决这个问题<br/>

    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn][26];
    int pc=1;
    //int coun[maxn];
    bool ok[maxn];
    int total;
    void bulidtree(string a)
    {
    int len=a.size();
    int root=0;
    fro(i,0,len)
    {
        int s=a[i]-'a';
        if(!tree[root][s])
            tree[root][s]=pc++;
        root=tree[root][s];
        //coun[root]++;
    }
    if(!ok[root])
    {
        total++;
        ok[root]=true;
    }
    //else
       // ok[root]=true;
    }
    int main()
    {
    string s;
    while(getline(cin,s))
    {
        mem(tree,0);
        mem(ok,false);
        pc=1;
        //mem(coun,0);
        if(s[0]=='#')
            break;
            total=0;
            string a;
        stringstream ss(s);
        while(ss>>a)
        {
            bulidtree(a);
        }
     cout<<total<<endl;
    }
    return 0;
    }

### POJ 字典树 2001 Shortest Prefixes
题目链接:<https://vjudge.net/problem/POJ-2001><br/>
通过判断布尔标记ok解决。<br/>

    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn][26];
    int coun[maxn];
    bool ok[maxn];
    int pc=1;
    string a[1001];
    void bulidtree(string s)
    {
    int len=s.size();
    int root=0;
    fro(i,0,len)
    {
        int ah=s[i]-'a';
        if(!tree[root][ah])
            tree[root][ah]=pc++;
        root=tree[root][ah];
        coun[root]++;
    }
    ok[root]=true;
    } 
    string query(string a)
    {
    int len=a.size();
    int root=0;
    int k=len;
    fro(i,0,len)
    {
        int ah=a[i]-'a';
        root=tree[root][ah];
        if(coun[root]==1)
        {
            k=i;
            break;
        }
    }
    return a.substr(0,k+1);
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n=0;
    while(cin>>a[n])
    {
        bulidtree(a[n]);
        n++;
    }
    fro(i,0,n)
    {
        string ss=query(a[i]);
        cout<<a[i]<<" "<<ss<<endl;
    }
    return 0;
    }


### 01字典树 hdu 4825 Xor Sum 异或运算模板
题目链接：<https://vjudge.net/problem/HDU-4825><br/>
01字典树，注意异或等位运算法则，通过看异或运算顺便复习了一下快速幂。<br/>
大佬教你了解位运算：<https://www.luogu.org/blog/chengni5673/er-jin-zhi-yu-wei-yun-suan><br/>

    const int maxn=3e6+10;
    const int INF=0x3f3f3f3f;
    int tree[maxn][2];
    ll coun[maxn];//计数
    int pc;//模拟结点
    //bool ok[maxn];//节点下面是否还有节点
    void bulidtree(ll a)
    {
    int root=0;
    for(int i=30;i>=0;i--)
    {
      int pos=(a>>i)&1;
      if(!tree[root][pos])
        tree[root][pos]=++pc;
      root=tree[root][pos];
    }
     coun[root]=a;//该二进制结点所存的数
    }
    ll query(ll a)
    {
    int root=0;
    for(int i=30;i>=0;i--)
    {
        int pos=(a>>i)&1;
        if(tree[root][pos^1])//异或最大值
        {
            root=tree[root][pos^1];
        }
        else
            root=tree[root][pos];
    }
    return coun[root];
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    //cin>>t;
    scanf("%d",&t);
    int case1=1;
    while(t--)
    {
        mem(tree,0);
        mem(coun,0);
        pc=0;
        int a,b;
        //cin>>a>>b;
        scanf("%d%d",&a,&b);
        fro(i,0,a)
        {
            ll s;
            scanf("%lld",&s);
            bulidtree(s);
        }
        //cout<<"Case #"<<case1++<<":"<<endl;
        printf("Case #%d:\n",case1++);
        fro(i,0,b)
        {
            ll s;
            scanf("%lld",&s);
            printf("%lld\n",query(s));
        }
    }
    return 0;
    }

### 字典树 POJ Phone List 3630
题目链接：<https://vjudge.net/problem/POJ-3630><br/>
re多次，最后发现是没有更新结点。。。。<br/>

![哪吒](/img/lz2.jpg)






