---
layout: post
title: 搜索+线段树（区间染色）
date: 2019-9-23
categories: blog
tags: [算法,搜索,线段树]
description: 语言
---
### B - Wang Xifeng's Little Plot HDU - 5024（bfs）
题目链接：<https://vjudge.net/contest/328694#problem/B><br/>
题目大意：给出一张图，要求从一个点'.'开始，可以走八个方向，要求只拐一个弯，且等于90度，求能走的最长距离。<br/>
思路：<br/>
初始思路，遍历整张图，找到'.'，分别bfs两次，分别是从上下左右走，和左上左下右上右下，关于90度问题，通过数组的位置取余解决，果不其然T了，我就知道.....，补题，看了题解，题解bfs一次，用数组记录8个方向，且数组中的位置的下标可由取余得到相应的90度的路的步数，整体思路就是把每个点看为拐点，走8个方向。<br/>


   char mas[101][101];
    int dirtion[8][2]={{0,-1},{-1,-1},{-1,0},{-1,1},{0,1},{1,1},{1,0},{1,-1}};
    int dis[10];
    int n;
    int bfs(int a,int b)
    {
    mem(dis,0);
    fro(i,0,8)
    {//将一个点枚举8个方向
        int x=a+dirtion[i][0];
        int y=b+dirtion[i][1];
        while(x>=0&&x<n&&y>=0&&y<n&&mas[x][y]!='#')
        {//一直走
            dis[i]++;
            x+=dirtion[i][0];
            y+=dirtion[i][1];
        }
    }
    int sum=0;
    fro(i,0,8)
    {
        sum=max(sum,dis[i]+dis[(i+2)%8]+1);//点睛之笔，方向安排得很巧妙，通过除余就可以得到
    }//对应的直角方向的值
    return sum;
    }
    int main()
    {
    ios::sync_with_stdio(false);
    while(cin>>n)
    {
       if(n==0)
        break;
        mem(dis,0);
       fro(i,0,n)
       {
           fro(j,0,n)
           cin>>mas[i][j];
       }
       int maxsum=0;
       fro(i,0,n)
         fro(j,0,n)
         {
             if(mas[i][j]=='.')//把每个点当为拐点
             {
                 maxsum=max(maxsum,bfs(i,j));
             }
         }
         cout<<maxsum<<endl;
    }
    }


### A - A Corrupt Mayor's Performance Art HDU - 5023 （线段树区间染色）
题目链接：<https://vjudge.net/contest/328694#problem/A><br/>
题目大意：给出n次操作加查询，P为改变数组的值，q为询问这个区间里有几种数并输出。<br/>
思路:询问加修改，哇这不是线段树马，可惜我想简单了，如果是线段树的话，那么tree数组维护什么呢？我之前遇到的多数是维护一个值或者和或最大值之类的，但这可是几个数啊，怎么弄呢，陷入沉思，知识盲区。。。。<br/>
这个时候位运算他来了，他来了，他来了....因为题目说颜色只能1到30，所以，可以用二进制表示（划重点）<br/>
比如：<br/>
1-----1<br/>
2-----10<br/>
3-----100<br/>
4-----1000<br/>
5-----10000<br/>
....<br/>
并且加上位运算的‘|’运算符，可以将两个二进制数相加，比如1+10=11，表示有1和2两种颜色，并且当一种颜色时，不改变值（这个就和强了，当然肯定tree数组的更新和懒标记的下传也会不一样，这个挺重要的,代码好好看看

    int tree[maxn<<2];
    int lazy[maxn<<2];
    void pushup(int rt)
    {
    tree[rt]=tree[rt<<1]|tree[rt<<1|1];
    return ;
    }
    void pushdown(int rt)
    {
    if(lazy[rt])
    {
        lazy[rt<<1]=lazy[rt<<1|1]=lazy[rt];
        tree[rt<<1]=tree[rt<<1|1]=lazy[rt];
        lazy[rt]=0;
    }
    }
    void bulidtree(int l,int r,int rt)
    {
    lazy[rt]=2;
    if(l==r)
    {
        tree[rt]=2;
        return;
    }
    int mid=l+(r-l)/2;
    bulidtree(ls);
    bulidtree(rs);
    pushup(rt);
    }
    void updata(int a,int b,int c,int l,int r,int rt)
    {
    if(a<=l&&b>=r)
    {
        tree[rt]=1<<(c-1);
        lazy[rt]=1<<(c-1);
        return;
    }
    pushdown(rt);//懒标记下传
    int mid=l+(r-l)/2;
    if(a<=mid)
        updata(a,b,c,ls);
    if(b>mid)
        updata(a,b,c,rs);
    pushup(rt);
    }
    int query(int a,int b,int l,int r,int rt)
    {
    if(a<=l&&b>=r)
    {
        return tree[rt];
    }
    int mid=l+(r-l)/2;
    pushdown(rt);
    int ans=0;
    if(a<=mid)
        ans|=query(a,b,ls);
    if(b>mid)
        ans|=query(a,b,rs);
    return ans;
    }
    int main()
    {
    //ios::sync_with_stdio(false);
    int m,n;
    while(cin>>m>>n)
    {
       if(!m&&!n)
        break;
       bulidtree(1,maxn,1);
       fro(i,0,n)
       {
           char a;
           int b,c,d;
           cin>>a;
           if(a=='P')
           {
               cin>>b>>c>>d;
               updata(b,c,d,1,maxn,1);
           }
           else
           {
               cin>>b>>c;
               int ans=query(b,c,1,maxn,1);
               bool ok=1;
               fro(i,1,30+1)
               {
                   if(ans&1)
                   {
                     if(ok)
                     {
                         cout<<i;
                         ok=0;
                     }
                     else
                        cout<<" "<<i;
                   }
                   ans>>=1;
               }
               cout<<endl;
           }
       }
    }
    }
    










