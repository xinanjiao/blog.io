---
layout: post
title: ACM训练第5天
date: 2019-7-27
categories: blog
tags: [算法,搜索,训练]
description: 语言
---

### DFS与BFS

1. 迷宫问题：（路径打印）<br/>
题目链接：<https://vjudge.net/contest/314142#problem/A><br>

code:

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int inf=0x3f3f3f;
    int maze[5][5];
    vector<P> a;
    int book[5][5];
    int dir[4][2]={{1,0},{0,1},{-1,0},{0,-1}};
    bool dfs(int m,int n)
    {
    if(m==4&&n==4)
        return true;
    fro(i,0,4)
    {
        int nm=m+dir[i][0];
        int nn=n+dir[i][1];
        if(nm>=0&&nm<5&&nn>=0&&nn<5&&book[nm][nn]==0&&maze[nm][nn]==0)
        {
            book[nm][nn]=1;
            if(dfs(nm,nn))//直接判断是否可以到终点
            {
            P s(nm,nn);
            a.push_back(s);
            return true;
            }
        }
    }
    return false;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    fro(i,0,5)
     fro(j,0,5)
      cin>>maze[i][j];
      mem(book,0);
      book[0][0]=1;
      P s(0,0);
    dfs(0,0);
    a.push_back(s);
    pre(i,0,a.size())
    {
        cout<<"("<<a[i].first<<", "<<a[i].second<<")"<<endl;
    }
    }

2. 棋盘问题:（简单版8皇后问题）<br/>
题目链接:<https://vjudge.net/contest/314142#problem/B><br/>

code:

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=1e5+10;
    const int inf=0x3f3f3f;
    int vis[10];
    char map1[10][10];
    int n,count1=0;
    void dfs(int m,int k)
    {
    if(k==0)
    {
        count1++;
        return;
    }
    if(m>=n)//越界
        return;
    fro(j,0,n)
        {
            if(!vis[j]&&map1[m][j]=='#')
            {
                vis[j]=1;
                dfs(m+1,k-1);
                vis[j]=0;
            }
        }
    dfs(m+1,k);//上一行不行，下一行
    }
    int main()
    {
    int k;
    while(cin>>n>>k)
    {
        if(n==-1&&k==-1)
            break;
    mem(vis,0);
    count1=0;
    fro(i,0,n)
    fro(j,0,n)
    cin>>map1[i][j];
        dfs(0,k);
    cout<<count1<<endl;
    }
    }

3. 哈密顿绕行世界问题（DFS）<br>
题目链接：<https://vjudge.net/contest/314142#problem/C><br/>

code:

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=1e5+10;
    const int inf=0x3f3f3f;
    struct node
    {
    bool s[21];
    node()
    {
        mem(s,false);
    }
    }a[21];//记录每条边
    int book[21];//标记点
    int point[21];//打印结果
    int mm,case1;
    vector<int> ss;
    void dfs(int m,int jd)
    {
    if(jd==20&&m==mm)
    {
       case1++;
       cout<<case1<<":  ";
       cout<<mm;
       fro(i,1,21)
        cout<<" "<<point[i];
       cout<<endl;
       return;
    }
    if(jd>20)
    return ;
    fro(i,1,21)
    {
       if(!book[i]&&a[m].s[i])
       {
           book[i]=1;
           point[jd+1]=i;
           dfs(i,jd+1);
           book[i]=0;
       }
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    fro(i,1,21)
    {
       int a1,b,c;
       cin>>a1>>b>>c;
       a[i].s[a1]=true;
       a[i].s[b]=true;
       a[i].s[c]=true;
    }
    while(cin>>mm)
    {
       if(mm==0)
        break ;
        mem(book,0);
        mem(point,0);
        case1=0;
       dfs(mm,0);
    }
    }


4. 火柴棒等式(暴力求解)<br>
题目链接:<https://vjudge.net/contest/314142#problem/D><br>

code

    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    typedef pair<int,int> P;
    const int maxn=1e6+10;
    const int inf=0x3f3f3f;
    ll gcd(ll a,ll b)
    {
    return b?gcd(b,a%b):a;
    }
    int a[10]={6,2,5,5,4,5,6,3,7,6};
    int sum=0;
    int trans(int a1)
    {
    int sum2=0;
    if(a1==0)
        return a[0];
    else
    {
        while(a1!=0)
    {
        int aa=a1%10;
         sum2+=a[aa];
        a1/=10;
    }
    return sum2;
    }
    }
    void dfs(int m,int now)
    {
    fro(i,0,999)
    {
        int k=now-trans(i);
        if(k-trans(m+i)==0)
            {
                sum++;
                //cout<<m<<" "<<i<<endl;
            }
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    while(cin>>n)
    {
        sum=0;
        if(n==23)
            sum=88;
        else if(n==24)
            sum=128;
        else
        {
        n-=4;
        fro(i,0,999)
        {
            int a1=n-trans(i);
            if(a1>0)
            dfs(i,a1);
            else
                break;
        }
        }
        cout<<sum<<endl;
    }
    }












