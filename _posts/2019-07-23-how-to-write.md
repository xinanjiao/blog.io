---
layout: post
title: ACM训练日记-第一天
date: 2019-7-23
categories: blog
tags: [算法,深度搜索,训练]
description: 语言
---
### 做题及题解
#### 个人赛
### Another Crosses Problem 
[题目链接](https://vjudge.net/problem/CodeForces-1194B)<br/>

**题目大意**<br/>
给你由字符组成的矩阵，让你判断由某种字符联通上下左右至少需要添加几个相同的字符<br/>

这是个人赛里面的第一道题，拿到时我一眼认为是DFS,后来我错辽。后来听他们讲实际上是模拟吧，只不过里面也有需要注意的地方，数组不要开得太大，不然也会TE<br/>

    #include <bits/stdc++.h>
    #include<cstring>
    using namespace std;
    const int inf=0x3f3f3f;
    const int maxn=5e4+10;
    const int maxr=4e5+10;
    int numi[maxn];
    int numj[maxn];
    int n,m;
    string a[maxn];
    int main()
    {
    ios::sync_with_stdio(false);//保证cin不会超时
    int nn;
    cin>>nn;
    while(nn--)
    {
        cin>>n>>m;
        memset(numi,0,sizeof(numi));
        memset(numj,0,sizeof(numj));
        for(int i=0;i<n;i++)
           {
              cin>>a[i];
            for(int j=0;j<m;j++)
           {
            if(a[i][j]=='.')
            {
                numi[i]++;
                numj[j]++;
            }
           }
           }
        int pos=inf;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                int sum=numi[i]+numj[j];
                if(a[i][j]=='.')
                    sum--;
                pos=min(pos,sum);
            }
        }
        cout<<pos<<endl;
    }
    return 0;
    }
 
  其中学到一点，就是INF是个很大的一个数，用0x3f3f3f表示很合适<br/>
  紫书中说cin输入流在一些需要输入数据大的程序中会超时，确实我遇到了，解决方法是加上
     ios::sync_with_stdio(false);
 
### world cup
[题目链接](http://codeforces.com/gym/101194/attachments)<br/>
**题目大意**<br/>
模拟世界杯的计分方式，4个队，共6场，赢一场得3分，平局个得1分，输了没有分数，给出四个最终分数，判断是否正确，如果有多种比赛情况得出的分数输出”NO“<br/>
刚开始不知道怎么做，打算暴力模拟出所有情况，苦于没有好的方法，也就没有做出了，结束后看他们的代码，受益匪浅。主要是状态的储存，开一个四维数组储存每种情况出现的次数，就这样暴力枚举所有情况就可以了<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <cmath>
    #include <deque>
    #include <vector>
    #include <queue>
    #include <string>
    #include <cstring>
    #include <map>
    #include <stack>
    #include <set>
    #include <sstream>
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    const int inf=0x3f3f3f;
    const int maxn=20;
    int vis[maxn][maxn][maxn][maxn];//计数（状态储存）
    void predeal()
    {
    mem(vis,0);
    int p2[3]={0,1,3};//负方得分
    int p1[3]={3,1,0};//胜方得分
    fro(ab,0,3) fro(ac,0,3) fro(ad,0,3)
    fro(bc,0,3) fro(bd,0,3) fro(cd,0,3)
    {
        int a=p1[ab]+p1[ac]+p1[ad];
        int b=p2[ab]+p1[bc]+p1[bd];
        int c=p2[ac]+p2[bc]+p1[cd];
        int d=p2[ad]+p2[bd]+p2[cd];
        vis[a][b][c][d]++;
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int la,lb,lc,ld,case1=1;
    int t;
    predeal();
    cin>>t;
    while(t--)
    {
        cin>>la>>lb>>lc>>ld;
        cout<<"Case #"<<case1++<<": ";
        if(vis[la][lb][lc][ld]==1)//有一次
            cout<<"Yes"<<endl;
        else if(vis[la][lb][lc][ld]>1)//大于一次
            cout<<"No"<<endl;
        else//没有出现过
          cout<<"Wrong Scoreboard"<<endl;
    }
    }
上面代码中define很漂亮，可以省去很多重复且不重要的代码，节约时间<br/>
### 排列
[题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=6235)<br/>
**题目大意**<br/>
给出一种规则，按这种规则排列数字。pi%(pi-pi-2)恒等于零(pi为第i个数字，pi-2为i-2个数字)<br/>
比赛中我是没看懂题（orz),果然理解能力有待提高。赛后重补这道题，思路也很复杂，看了看题解，想要恒为0，只要除余1就行了(orz),所以把数字间隔为2排列<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <algorithm>
    #include <cmath>
    #include <deque>
    #include <vector>
    #include <queue>
    #include <string>
    #include <cstring>
    #include <map>
    #include <stack>
    #include <set>
    #include <sstream>
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    const int inf=0x3f3f3f;
    const int maxn=3e5+10;
    int a[maxn],b[maxn];
    int main()
    {
    ios::sync_with_stdio(false);
    mem(a,0);mem(b,0);
    int t;
    cin>>t;
    while(t--)
    {
        int n;
        cin>>n;
        int len=(n+1)/2;
        int p=1;//只要间隔中相差为1，则除余恒为0
        for(int i=1;i<=n;i+=2)
           {
               a[i]=p;
               p++;
           }
           for(int j=2;j<=n;j+=2)
           {
               a[j]=p;
               p++;
           }
           for(int i=1;i<=n;i++)
            cout<<a[i]<<" ";
    }
    }
在大佬代码里还可以学到不少东西<br/>
### icetower
[题目链接](http://codeforces.com/gym/101194/attachments)<br/>
**题目大意**
给出一种规则，最下层的冰淇淋大于等于上一层的冰淇淋2倍，给出N种冰淇淋，和需要M层，判断这些冰淇淋能叠的最高层数<br/>
表面为贪心，每次选最大的冰淇淋放在最下面，但又和普通贪心不一样，因为这样不一定得到最优解，所以选用二分法+贪心，二分法枚举答案，正难则反，通过答案来枚举是否能得到这个解。<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <algorithm>
    #include <cmath>
    #include <deque>
    #include <vector>
    #include <queue>
    #include <string>
    #include <cstring>
    #include <map>
    #include <stack>
    #include <set>
    #include <sstream>
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    const int inf=0x3f3f3f;
    const int maxn=3e5+10;
    ll s[maxn],ans[maxn];
    ll k,n;
    bool check(ll m)//根据答案来查找是否符合（贪心）
    {
    ll p=0;
    mem(ans,0);
    fro(i,1,m+1)
    {
        ans[i]=s[++p];//把最小的m个数加入ans判断数组
    }
    fro(i,1,k)
    {//共k层
        fro(j,1,m+1)
        {
            p++;
            while(p<=n&&s[p]<ans[j]*2)p++;//从p开始匹配后面的ice
            if(p>n)
                return 0;
            ans[j]=s[p];
        }
    }
    return 1;
    }
    int main()
    {
    ll t;
    ll a;
     ll case1=1;
     cin>>t;
    while(t--)
    {
       cin>>n>>k;
       fro(i,1,n+1)
       {
           cin>>a;
           s[i]=a;
       }
       sort(s+1,s+n+1);
       ll r=n/k,l=0;
       ll res=0;
       while(l<=r)//二分查找答案
       {
           ll mid=(r+l)/2;
           if(check(mid))
            {
                res=mid;
                l=mid+1;
            }
           else
            r=mid-1;
       }
       cout<<"Case #"<<case1++<<": "<<res<<endl;
    }
    }
二分查找实际上是个模板，可以记住！<br/>


## DFS专练



### Red and Black (POJ 1979)
[题目链接](https://vjudge.net/problem/POJ-1979#author=0)<br/>
**题目大意**<br/>
有三种符号组成的矩阵，其中'@'表示这个人起始位置，判断这个人能连续通过的瓷砖数（‘.’表示瓷砖）<br/>
显然是深搜<br/>

    #include<iostream>
    #include<cstring>
    #include<queue>
    using namespace std;
    const int inf=0x3f3f3f;
    char map1[21][21];
    int m,n;
    int sm,sn;
    int sum=0;
    int book[21][21];
    int dir[4][2]={1,0,-1,0,0,1,0,-1};
    void dfs(int sm,int sn)
    {
    book[sm][sn]=1;
    for(int i=0;i<4;i++)
    {
        int nm=sm+dir[i][0];
        int nn=sn+dir[i][1];
        if(nm>=0&&nm<m&&nn>=0&&nn<n&&map1[nm][nn]=='.'&&book[nm][nn]==0)
        {
            sum++;
            dfs(nm,nn);
        }
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    while(cin>>n>>m)
    {
        if(m==0&&n==0)
            break;
        sum=0;
        memset(book,0,sizeof(book));
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
                cin>>map1[i][j];
        }
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(map1[i][j]=='@')
                   {
                       sm=i;sn=j;
                       sum++;
                   }
            }
        }
        dfs(sm,sn);
        cout<<sum<<endl;
    }
    }
### Property Distribution (AOJ 0118)
[题目链接](https://vjudge.net/problem/Aizu-0118#author=0)<br/>
/***题目大意**<br/>*/
有四种符号组成的矩阵，判断有多少四联通块,染色+dfs<br/>

    #include <iostream>
    #include <cstdio>
    using namespace std;
    const int inf=0x3f3f3f;
    char map1[110][110];
    int dir[4][2]={{0,1},{0,-1},{1,0},{-1,0}};
    char ch;
    int sum;
    int h,w;
    void dfs(int m,int n,char ch)
    {
    map1[m][n]='.';
    for(int i=0;i<4;i++)
    {
        int hm=m+dir[i][0];
        int hn=n+dir[i][1];
        if(hm>=0&&hm<h&&hn>=0&&hn<w&&map1[hm][hn]==ch)
        {
            map1[hm][hn]='.';
            dfs(hm,hn,ch);
        }
    }
    }
    int main()
    {
    while(cin>>h>>w)
    {
        if(h==0&&w==0)
            break;
            sum=0;
        for(int i=0;i<h;i++)
         for(int j=0;j<w;j++)
         cin>>map1[i][j];
        for(int i=0;i<h;i++)
        {
            for(int j=0;j<w;j++)
            {
                if(map1[i][j]=='*'||map1[i][j]=='#'||map1[i][j]=='@')
                    {
                        ch=map1[i][j];
                        dfs(i,j,ch);
                        sum++;
                    }
            }
        }
        cout<<sum<<endl;
    }
    }

### ball (AOJ 0033)
[题目链接](https://vjudge.net/problem/Aizu-0033#author=0)<br/>
**题目大意**<br/>
从1到10连续编号的小球从容器开口A放入。通过调整枢轴E的方向，可以使小球经过D而落入左侧B筒或者右侧C筒。现给出从A放入小球的顺序，请你判断能否最终小球落入B和C时，号码大的球总是位于号码小的球的上侧。如果可能则在一行中输出”YES”，否则输出 “NO”<br/>
<br/>
DFS巧用，实际上不用把落下的小球存起来，只要判断它是否大于前面一个数即可，原来我想多了，不用回溯，不行就是不行，不用考虑其他情况<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <algorithm>
    #include <cmath>
    #include <deque>
    #include <vector>
    #include <queue>
    #include <string>
    #include <cstring>
    #include <map>
    #include <stack>
    #include <set>
    #include <sstream>
    using namespace std;
    const int inf=0x3f3f3f;
    const int maxn=1010;
    int ball[maxn];
    bool ok;
    void dfs(int x,int r,int l)
    {
    if(x==10)
        return;
    if(ball[x]>r)
        dfs(x+1,ball[x],l);
    else if(ball[x]>l)
        dfs(x+1,r,ball[x]);
    else
    {
        ok=true;
        return;
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    int n;
    cin>>n;
    while(n--)
    {
        for(int i=0;i<10;i++)
            cin>>ball[i];
        ok=false;
        dfs(0,0,0);
        if(!ok)
            cout<<"YES"<<endl;
        else
            cout<<"NO"<<endl;
    }
    }
通过判断前面一个小球的大小，实现DFS的巧用<br/>
### Curling 2.0 (POJ 3009)
[题目链接](https://vjudge.net/problem/POJ-3009)<br/>
**题目大意**<br/>
小球在给出的矩阵中运动，规则如下：运动为四个方向，且一旦运动并会一直运动下去直到碰到障碍物，碰到障碍物，障碍物会消失，且这时小球必须改变方向，这时只有3个方向，最终到达目的地则结束，如果小球出界和超过10步则失败。<br/>
典型DFS回溯，注意回溯是步数减一，标记的点将回到最初状态<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <algorithm>
    #include <cmath>
    #include <deque>
    #include <vector>
    #include <queue>
    #include <string>
    #include <cstring>
    #include <map>
    #include <stack>
    #include <set>
    #include <sstream>
    using namespace std;
    const int inf=0x3f3f3f;
    char map1[21][21];
    int m,n;
    int sm,sn;
    int endm,endn;
    int sum=0;
    int book[21][21];
    int dir[4][2]={1,0,-1,0,0,1,0,-1};
    bool flag=true;
    int step=0;
    int ans;//储存比较最小步数
     void dfs(int sm,int sn,int step)
    {
    if(sm==endm&&sn==endn)
    {
        ans=min(ans,step);
        return;
    }
    if(step>10||step>=ans)
        return;
    for(int i=0;i<4;i++)
    {
        int nm=sm+dir[i][0];
        int nn=sn+dir[i][1];
        while(nm>=0&&nm<m&&nn>=0&&nn<n&&map1[nm][nn]!='1')
        {
            if(nm==endm&&nn==endn)
                {
                    step++;
                    ans=min(ans,step);
                    return;
                }
            nm+=dir[i][0];
            nn+=dir[i][1];
        }
        if(nm<0||nm>=m||nn<0||nn>=n||(nm==sm+dir[i][0]&&nn==sn+dir[i][1]))//出界或方向重复
            continue;
            step++;
          map1[nm][nn]='0';
          dfs(nm-dir[i][0],nn-dir[i][1],step);
          step--;//回溯
          map1[nm][nn]='1';
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    //ofstream out;
    //out.open("output.txt");
    while(cin>>n>>m)
    {
        if(m==0&&n==0)
            break;
        sum=0;
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
                cin>>map1[i][j];
        }
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(map1[i][j]=='2')
                   {
                       sm=i;sn=j;
                       map1[i][j]='0';//改掉起点
                   }
                   if(map1[i][j]=='3')
                   {
                       endm=i;endn=j;
                   }
            }
        }
        ans=11;
        dfs(sm,sn,0);
        if(ans==11)
         cout<<-1<<endl;
        else
          cout<<ans<<endl;
    }
    //out.close();
    }










