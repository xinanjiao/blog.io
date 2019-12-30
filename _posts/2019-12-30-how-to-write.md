---
layout: post
title: uva12333+1605
date: 2019-12-30
categories: blog
tags: [字典树,中途相遇法,数据结构]
description: 文章金句。
---

### uva 12333 Revenge of Fibonacci 大数+字典树

#### 题目大意：
斐波那契数列我们都早已熟悉，有一天，你正在睡觉，然后斐波那契数列变成一个老人前来复仇，它一步步勾引你，到最后，它说了一串数，是斐波那契数，然而直说前几个数，然后你又很感兴趣，问问你，能否找到以这一串数为开头的最小斐波那契数。

#### 数据范围

输入的前缀字符串长度 n <= 40,当查找的数大于100000时，输出-1。

    输出样例
    15
    1
    12
    123
    1234
    12345
    9
    98
    987
    9876
    98765
    89
    32
    51075176167176176176
    347746739
    5610 
    输出样例
    Case #1: 0 
    Case #2: 25 
    Case #3: 226 
    Case #4: 1628 
    Case #5: 49516 
    Case #6: 15 
    Case #7: 15 
    Case #8: 15 
    Case #9: 43764 
    Case #10: 49750 
    Case #11: 10 
    Case #12: 51 
    Case #13: -1 
    Case #14: 1233 
    Case #15: 22374


解题思路：我们都知道当斐波那契数大于64时，long long int 都存不下。所以就只能用大数来存储，即要用字符串来进行运算，那就要重载假发运算符，让最后的结果也为字符串。然后用字典树存储加查找。第一次我打算用字典树的，发现写到最后有点卡壳，有个地方不好实现，用map去查找了一发，T了，找到最后，实际上我的重载高精度加法很复杂，用的string，用string的话无法用数组形式的地址赋值，只能push_back(),这样就多了很多无意义的翻转操作，导致超时，最后写了字典树过辽，<p style="color: red">有个小插曲就是，当你把100000的值也压进字典树时就会错误！</p>，我打算有时间试试map会不会超时，毕竟有些字典树能做的是，map也能高效的完成！

    const int maxn = 6e6+100;
    int lowbit(int x){return x&(-x);}
    class cstring
    {
    public:
    char s[100];
    cstring(){}
    cstring operator +(cstring other)
    {
        int len1=strlen(s),len2=strlen(other.s);
        cstring aa;
        char ans[1000],ans2[1000];
        int i=len1-1,j=len2-1,g=0,cnt=0;
        while(i>=0||j>=0)
        {
            int x=g;
            if(i>=0)
            {
                x+=(s[i]-'0');
                i--;
            }
            if(j>=0)
                {
                    x+=(other.s[j]-'0');
                    j--;
                }
            ans[cnt++]=x%10+'0';
            g=x/10;
        }
        if(g>0)
            ans[cnt++]=g+'0';
        ans[cnt]='\0';
        int cnt2=0;
        for(int i=cnt-1;i>=0;i--)
        ans2[cnt2++]=ans[i];
        ans2[cnt2]='\0';
        strcpy(aa.s,ans2);
        return aa;
    }
    void operator = (cstring b)
    {
      strcpy(s,b.s);
    }
    };
    struct node
    {
    int id;
    int pc;
    node(){id=INF;pc=0;}
    };
    cstring a[maxn];
    node tree[maxn][11];
    int pc;
    void buildtree(char *a,int tt)
    {
    int root=0;
    int len=strlen(a);
    fro(i,0,len)
    {
        int t=a[i]-'0';
        if(!tree[root][t].pc)
            {
            tree[root][t].pc=++pc;
            tree[root][t].id=min(tree[root][t].id,tt);
            //cout<<tree[root][t].id<<endl;
            }
        root=tree[root][t].pc;
    }
     //cout<<pc<<endl;
    }
    int Search(string aa)
    {
    int len=aa.size();
    int root=0,now;
    fro(i,0,len)
    {
        int t=aa[i]-'0';
        if(!tree[root][t].pc)
            return -1;
            now=tree[root][t].id;
        root=tree[root][t].pc;
    }
    return now;
    }
    void init()
    {
    a[0].s[0]='1',a[1].s[0]='1';
    buildtree(a[0].s,0);
    for(int i=2;i<100000;i++)
    {
        a[i]=a[i-1]+a[i-2];
        buildtree(a[i].s,i);
        int len=strlen(a[i].s)-1;
        if(len>=60)
            a[i].s[len]=0,a[i-1].s[strlen(a[i-1].s)-1]=0;
       // cout<<a[i].s<<endl;
    }
    }
    int main()
    {
    ios::sync_with_stdio(0);
     init();
    //cout<<a[56].s<<endl;
    // cout<<a[1628].s<<endl;
    int t;
    while(cin>>t)
    {
    int case1=1;
    while(t--)
    {
        string a;
        cin>>a;
        cout<<"Case #"<<case1++<<": "<<Search(a)<<endl;
    }
    }
    return 0;
    }


### uva1605 Building for UN 中途相遇法

#### 题目大意：
给联合国设计大楼，给你n个国家，满足以下要求，每个国家要求都要相邻（同一层国家的小方格边相邻，不同层的国家即每层对应的地方相同），每个国家用不同的英文大小写区分，输出所设计的大楼每层平面的长，宽，高，最后输出每层的平面图。

<p style="color: red;font-size: 20px">中途相遇法</p>
<p style="color: red"> 这是一种特殊的算法，大体思路就是从两个不同的方向来解决问题，最终“汇聚”到一起，双向广度优先搜索方法就有一点中途相遇法的味道。</p>

方法：一共两层，每层都是n * n,第一层第i行全是国家i,第二层第j列全是国家j。

    const int maxn = 1e5+100;
    int lowbit(int x){return x&(-x);}
    int main()
    {
    ios::sync_with_stdio(0);
     string s="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
     int n;
     while(cin>>n)
     {
         cout<<2<<" "<<n<<" "<<n<<endl;
     fro(i,0,n)
     {
         fro(j,0,n)
          cout<<s[i];
        cout<<endl;
     }
     cout<<endl;
     fro(i,0,n)
     {
         fro(j,0,n)
         cout<<s[j];
         cout<<endl;
     }
     }
    return 0;
    }



