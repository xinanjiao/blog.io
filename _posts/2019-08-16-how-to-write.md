---
layout: post
title: ACM训练第18天（kmp)
date: 2019-8-16
categories: blog
tags: [算法,训练,kmp]
description: 语言
---
### 写在前面
没有晋级b组，唉，有点失落，还有两场了，抓紧机会冲啊！！<br/>
今天的专题是字符串，说到字符串，字典树可以运用到字符串，今天温习了（重学了）字符串匹配算法KMP,复杂度o(m+n)的算法。<br/>
算法难吗？肯定难啊，难就不学了？不可能的，比起学算法，题目AC的那一刻，是最开心的，所以加油加油加油，冲冲冲。


### 今日份get---KMP
大佬博客（超详细）：<https://blog.csdn.net/v_july_v/article/details/7041827><br/>
<p style="color: red;">kmp算法，是bf算法（暴力匹配法）的升级版，它不再是单纯的暴力一个个匹配，成功加一，失败在回溯，而是用一个数组（next数组）存储状态，next数组也叫前后缀数组，它存储的是这个字符串重复的前后缀，这使得不是每次匹配失败都回溯到上一个，而是通过next数组来判断回溯到正确的位置，这样节约了不少时间。next数组的求法也可以优化。</p>


## 题目（代码后补）
今天题不多，主要是把重要的几个题记录一下


### kmp优化 字符串匹配 B - Oulipo
题目链接：<https://vjudge.net/contest/320131#problem/B><br/>
一个模式串和一个主串，求主串中有几个模式串。一个思路，暴力枚举，一个个模拟，绝对超时。用到算法，kmp，高效解决字符串匹配问题<br/>

    int sum;
    int next1[10010];
    void get_next(string a)
    {
    int len=a.size();
    next1[0]=-1;
    int k=-1;
    int j=0;
    while(j<len)
    {
        if(k==-1||a[k]==a[j])
        {
            j++;
            k++;
            if(a[k]!=a[j])
            next1[j]=k;
            else
                next1[j]=next1[k];
        }
        else
        k=next1[k];
    }
    }
    void kmp(string a,string b)
    {
    int l=0,r=0;
    int len1=a.size(),len2=b.size();
    while(l<len1)
    {
        if(r==-1||a[l]==b[r])
           {
               l++;
                r++;
           }
        else
            r=next1[r];
        if(r==len2)
        {
            r=next1[r];
            sum++;
        }
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int t;
    cin>>t;
    while(t--)
    {
        sum=0;
        mem(next1,0);
        string s1,s2;
        cin>>s1;
        get_next(s1);
        cin>>s2;
        kmp(s2,s1);
        cout<<sum<<endl;
    }
    return 0;
    }

### A - Encoded Barcodes 字典树
题目链接：<https://vjudge.net/contest/313327#problem/A><br/>
一眼字典树，建树和查询都是模板，没什么好说的，主要是查询的字符串需要数字解码而来，我先一直没有看懂解码过程，解码就将数字对应为0或1，在将它看成二进制，再转为十进制。<br/>

    int tree[maxn][26];
    int coun[maxn];
    //bool ok[maxn];
     int pc=1;
     char a[1001];
    double s[8];
    double sums;
    void buildtree(char *a)
    {
    int root=0;
    int len=strlen(a);
    fro(i,0,len)
    {
        int s=a[i]-'a';
        if(!tree[root][s])
         tree[root][s]=pc++;
         root=tree[root][s];
         coun[root]++;
    }
    // ok[root]=true;
    }
    int query(vector<char> a)
    {
    int len=a.size();
    int root=0;
    fro(i,0,len)
    {
        int s=a[i]-'a';
        if(!tree[root][s])
            return 0;
        root=tree[root][s];
    }
    return coun[root];
    }
    char change()
    {
    int sum=0;
    fro(i,0,8)
    {
        int aa;
        if(s[i]>sums)
            aa=1;
        else
            aa=0;
        sum+=(aa*pow(2,8-i-1));
    }
    char a=sum;
    return a;
    }
    int main()
    {
    //ios::sync_with_stdio(false);
     int m,n;
     while(scanf("%d%d",&m,&n)!=EOF)
     {
         mem(tree,0);
         mem(coun,0);
         //mem(ok,false);
         pc=1;
         fro(i,0,m)
         {
             scanf("%s",a);
             buildtree(a);
         }
          int vis=0;
         fro(i,0,n)
         {
             int ss;
            vector<char> v;
             scanf("%d",&ss);
             fro(i,0,ss)
             {
                 sums=0;
                 fro(k,0,8)
                 {
                     scanf("%lf",&s[k]);
                     sums+=s[k];
                 }
                 sums/=8.0;
                v.push_back(change());
                //cout<<change()<<endl;
             }
             vis+=query(v);
             //cout<<query(v)<<endl;
         }
        printf("%d\n",vis);
     }
    return 0;
    }

### D - Zuhair and Strings  暴力枚举
题目链接：<https://vjudge.net/contest/313340#problem/D><br/>
看懂题意简单，前提是看懂（哭了），枚举26个字母，重头到尾扫一遍就行了<br/>

    const int maxn=2e5+10;
    const int INF=0x3f3f3f3f;
    char a[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    int m,n;
    cin>>m>>n;
    fro(i,0,m)
    {
        cin>>a[i];
    }
    int maxs=-INF;
    fro(i,0,26)
    {
        char s=i+'a';
        int sum=0,vis=0;
        fro(i,0,m)
        {
            if(s==a[i])
            {
                sum++;
            }
            else
                sum=0;
            if(sum==n)
            {
                vis++;
                sum=0;
            }
        }
        maxs=max(maxs,vis);
    }
    cout<<maxs<<endl;
    return 0;
    }

![哪吒](/img/lz5.jpg)




