---
layout: post
title: ACM训练第19天(马拉车算法）
date: 2019-8-17
categories: blog
tags: [算法,马拉车算法,训练,kmp]
description: 语言
---

### 写在前面
字符串博大精深，训练也接近尾声，今天主要是补题，搞算法。嗯，就酱紫....加油

### 今日份get---Manacher算法（马拉车算法）
起初听到这个名字我还纳闷，是不是像尺取法一样通过算法运行像马拉车一样来命名这个名字，但是了解了以后完全不沾边，看了这个发明人名字后恍然大悟，ma na che 哈哈哈，中华文化博大精深啊，哈哈哈。学了一下午还是不太懂，有点抽象，后面还得深入研究研究。<br/>
马拉车我认为讲的很好的两个链接：<br/>
<https://blog.csdn.net/HappyRocking/article/details/82622881><br/>
<https://blog.csdn.net/the_star_is_at/article/details/53354958><br/>
<p style="color: red;">马拉车算法，是一个专注于求最长回文的算法。我们以前最普通的求字符串回文的算法复杂度是o（n3),可以优化后是o(n2),但这个算法复杂度仅为o(n)。完美根据回文串的特性。操作基本分为以下几步：1.添加。往字符串里面添加不会造成影响的字符，最后需要在字符串首部添加另外一个字符，保证从数组1位开始。这个操作是为了让每个回文串的中心都是奇数。2.回文半径数组，详情可以参见上面讲解。在每次最大半径小于回文串的半径是，都要更新回文串的半径和回文串的中心。</p>

## 题目
kmp next 数组 ，与马拉车

### C - Simpsons’ Hidden Talents next数组求最大前后缀
题目链接：<https://vjudge.net/contest/320131#problem/C><br/>
解题核心是把两个数组合并为一个，再用kmpnext数组的特殊性质求解<br/>

    using namespace std;
    #define ll long long  int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    ll gcd(ll a,ll b) {return b?gcd(b,a%b):a;}
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=1e5+10;
    const int INF=0x3f3f3f3f;
    int next1[maxn];
    int maxa;
    string suf;
    void get_next(string a)
    {
    //cout<<"i am here"<<endl;
    int len=a.size();
    next1[0]=-1;
    int k=-1;
    int j=0;
    while(j<len)
    {
        if(k==-1||a[j]==a[k])
        {
            j++;
            k++;
            if(a[j]!=a[k])
                next1[j]=k;
            else
                next1[j]=next1[k];
        }
        else
            k=next1[k];
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    string s1,s2;
    while(cin>>s1>>s2)
    {
        string s3=s1+s2;
        get_next(s3);
        int minlen=min(s1.size(),s2.size());
        int len=s3.size();
        if(next1[len]==0)
            cout<<0<<endl;
        else
            {
                if(minlen<=next1[len])
                    next1[len]=minlen;
               fro(i,0,next1[len])
               {
                   cout<<s3[i];
               }
               cout<<" "<<next1[len]<<endl;
            }
    }
    return 0;
    }

### D - 最长回文 马拉车算法模板
题目链接:<https://vjudge.net/contest/320131#problem/D><br/>
马拉车算法旨在解决一个字符串中最大回文串问题，与其他算法不同的是，他的复杂度仅为o(n)，线性复杂度，经典中的经典<br/>

    using namespace std;
    #define ll long long  int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    ll gcd(ll a,ll b) {return b?gcd(b,a%b):a;}
    const double PI = 3.1415926535897932;
    const double EPS=1e-6;
    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    char a[maxn];
    int p[maxn];
    char newa[maxn<<1];
    int change(char *a)
    {
    int len=strlen(a);
    newa[0]='$';
    newa[1]='#';
    int j=2;
    fro(i,0,len)
    {
        newa[j++]=a[i];
        newa[j++]='#';
    }
    newa[j]='\0';
    return j;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    while(cin>>a)
    {
       int len=change(a);
       int mx=0,id;
       int maxa=-1;
       fro(i,1,len)
       {
           if(mx>i)
            p[i]=min(p[2*id-i],(int)(mx-i));
           else
            p[i]=1;
           while(newa[i-p[i]]==newa[i+p[i]])
            p[i]++;
           if(mx<i+p[i])
           {
               id=i;
               mx=i+p[i];
           }
           maxa=max(maxa,p[i]-1);
       }
       cout<<maxa<<endl;
    }
    return 0;
    }

### B - Period 循环节 next 数组
题目链接：<https://vjudge.net/contest/313340#problem/B><br/>
next数组之再升级，详情请了解下面这篇关于循环节的博客<br/>
循环节：<https://www.cnblogs.com/chenxiwenruo/p/3546457.html><br/>










