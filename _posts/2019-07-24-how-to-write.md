---
layout: post
title: ACM训练第二天
date: 2019-7-24
categories: blog
tags: [算法,广度搜索,训练]
description: 文章金句。
---
Property Distribution (AOJ 0118)<br>
有四种符号组成的矩阵，判断有多少四联通块,染色+dfs<br>

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
题目链接:<https://vjudge.net/problem/Aizu-0033#author=0><br/>
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
题目链接:<https://vjudge.net/problem/POJ-3009><br/>
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

## BFS专练

### Cheese (AOJ 0558)
题目链接:<https://vjudge.net/problem/Aizu-0558><br/>
**题目大意**<br/>
在H * W的地图上有N个奶酪工厂，每个工厂分别生产硬度为1-N的奶酪。有一只老鼠准备从出发点吃遍每一个工厂的奶酪。老鼠有一个体力值，初始时为1，每吃一个工厂的奶酪体力值增加1（每个工厂只能吃一次），且老鼠只能吃硬度不大于当前体力值的奶酪。 老鼠从当前格到上下左右相邻的无障碍物的格需要时间1单位，有障碍物的格不能走。走到工厂上时即可吃到该工厂的奶酪，吃奶酪时间不计。问吃遍所有奶酪最少用时<br/>
需要思维转化，原来我就是一股脑的模拟，吃一个奶酪加一滴血，这样还需要回溯，很麻烦，本题最好的方法就是把从起点开始标号从0到N，每次就BFS一个点到另一个点的最短距离，如果全部一起的话，BFS恐怕不行。<br/>

    #include <iostream>
    #include <cstdio>
    #include <fstream>
    #include <algorithm>
    #include <cmath>
    using namespace std;
    #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    const int inf=0x3f3f3f;
    char map1[1001][1001];
    int dir[4][2]={{1,0},{0,1},{0,-1},{-1,0}};
    int m,n,sum,step=0;
    int book[1001][1001];
    struct factory//工厂
    {
    int x,y,step;
    };
    const int maxn=1e3;
    factory ss[maxn];
    int bfs(factory a,factory b)
    {
    mem(book,0);
    factory fir;
    fir.x=a.x;fir.y=a.y;fir.step=a.step;
    book[a.x][a.y]=1;
    queue<factory> s;
    s.push(fir);
    while(!s.empty())
    {
        factory want;
        want=s.front();s.pop();
        if(want.x==b.x&&want.y==b.y)
            return want.step;
        fro(i,0,4)
        {
            int nx=want.x+dir[i][0];
            int ny=want.y+dir[i][1];
            int step=want.step+1;
            if(nx>=0&&nx<m&&ny>=0&&ny<n&&!book[nx][ny]&&map1[nx][ny]!='X')
            {
                book[nx][ny]=1;
                fir.x=nx;fir.y=ny;fir.step=step;
                s.push(fir);
            }
        }
    }
    }
    int main()
    {
    ios::sync_with_stdio(false);
    cin>>m>>n>>sum;
       fro(i,0,m)
     {
         fro(j,0,n)
          {
              cin>>map1[i][j];
              if(map1[i][j]=='S')
                map1[i][j]='0';
              fro(k,0,sum+1)//记下工厂
              {
                  if((map1[i][j]-'0')==k)
                  {
                      ss[k].x=i;
                      ss[k].y=j;
                      ss[k].step=0;
                  }
              }
          }
     }
     int asum=0;
     fro(i,0,sum)
     {
         asum+=bfs(ss[i],ss[i+1]);
     }
     cout<<asum<<endl;
    }

### Meteor Shower (POJ 3669)
题目链接：<https://vjudge.net/problem/POJ-3669><br/>
**题目大意**<br/>
Bessie听说有场史无前例的流星雨即将来临；有谶言：陨星将落，徒留灰烬。为保生机，她誓将找寻安全之所（永避星坠之地）。目前她正在平面坐标系的原点放牧，打算在群星断其生路前转移至安全地点。
Bessie在0时刻时处于原点，且只能行于第一象限，以平行与坐标轴每秒一个单位长度的速度奔走于未被毁坏的相邻（通常为4）点上。在某点被摧毁的刹那及其往后的时刻，她都无法进入该点。

寻找Bessie到达安全地点所需的最短时间。<br/>
BFS,我当初也一直模拟发现判断太多了，网上给出一种很好的方法，预处理，把流星摧毁的地区标记好，地区的值是流星到达那里的时间，先把它全部枚举出来，后续判断就不会很复杂(orz)。保存状态<br>

    #include <iostream>
    using namespace std;
     #define ll long long int
    #define fro(i,a,n) for(ll i=a;i<n;i++)
    #define pre(i,a,n) for(ll i=n-1;i>=a;i--)
    #define mem(a,b) memset(a,b,sizeof(a))
    const int inf=0x3f3f3f;
    const int maxn=400;
    const int maxr=5e4+10;
    int map1[maxn][maxn];
    int book[maxn][maxn];
    int n;
    int time2=0;
    int dir[5][2]={{1,0},{0,1},{-1,0},{0,-1},{0,0}};
    struct bome
    {
      int x,y;
    int time;
    }bome1[maxr];
    struct girl
    {
    int x;
    int y;
    int time2;
    }a,b,c;
    bool cmp(bome a,bome b)
    {
    return a.time<b.time;
    }
    int bfs()
    {
    a.x=0;a.y=0;
    a.time2=0;
    queue<girl> s;
    s.push(a);
    book[a.x][a.y]=1;
    while(!s.empty())
    {
        b=s.front();
        s.pop();
        fro(i,0,4)//女孩移动
        {
            a.x=b.x+dir[i][0];
            a.y=b.y+dir[i][1];
            a.time2=b.time2+1;
            if(a.x>=0&&a.y>=0&&(map1[a.x][a.y]>a.time2||map1[a.x][a.y]==-1)&&!book[a.x][a.y])
            {
                book[a.x][a.y]=1;
                s.push(a);
                if(map1[a.x][a.y]==-1)
                {
                    return a.time2;
                }
            }
        }
    }
    return -1;
    }
    int main()
    {
    cin>>n;
    fro(i,0,n)
    {
        cin>>bome1[i].x>>bome1[i].y>>bome1[i].time;
    }
    mem(map1,-1);mem(book,0);
    sort(bome1,bome1+n,cmp);
    fro(i,0,n)
        {
        int t=bome1[i].time;
        fro(j,0,5)//四周摧毁
        {
            if((bome1[i].x+dir[j][0]>=0)&&(bome1[i].y+dir[j][1]>=0)&&(t<=map1[bome1[i].x+dir[j][0]][bome1[i].y+dir[j][1]]||map1[bome1[i].x+dir[j][0]][bome1[i].y+dir[j][1]]==-1))
                {
                map1[bome1[i].x+dir[j][0]][bome1[i].y+dir[j][1]]=bome1[i].time;
                }
        }
        }
        cout<<bfs()<<endl;
    }











