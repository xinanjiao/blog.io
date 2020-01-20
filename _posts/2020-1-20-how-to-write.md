---
layout: post
title: codeforce div614+uva 1615 贪心
date: 2020-1-20
categories: blog
tags: [贪心,codeforce]
description: 语言
---

### 快过年了
<p style="color: red">我是一个喜欢感慨的人。就拿大一来说吧，看见啥好句子，看见啥大道理，迫不及待想让别人知道，表现就是经常发说说。我以前一个同学说过，经常发说说的人，实际上是很孤独的，想想也确实，大一那会儿，在青岛人生地不熟的，确实孤独。觉得自己慢慢开始接受更多的思想，更多的道理，更多听起来糊涂却很现实的东西。知世故而不世故，慢慢成长</p>

<p style="color: red">快过年了，却一点年味都没有。‘小孩子才有过年，大人就只有忙里忙外，安排每天的行程，推让红包’，在大人眼中，过年只是个流程罢了。看来我也大了，感受不到啥年味了。那些该来的，该走的我们也挡不住。但我可以觉得我未来可以遇到哪些人，或穿梭于写字楼的白领，或生活与集居房的普通人。我能决定我的未来！</p>

## codefroce div2 614

### A - ConneR and the A.R.C. Markland-N CodeForces - 1293A 
### 题目大意
阅读题，给出1到n个房间，现在在s房间，有k间房间关着，问离最近的开着的房间号需要最少的距离，没有就输出0.

### 思考
说来惭愧，没看见居然可以在一间房间吃饭，也就是说s这个房间开着，那就输出0

    int a[1100];

    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        mem(a,0);
        map<int,int> mp;
        int n,s,k;
        cin>>n>>s>>k;
        fro(i,0,k){
        cin>>a[i];
        mp[a[i]]=1;
        }
        if(!mp[s]){
            cout<<0<<endl;
        }
        else{
        int sum=0;
        //sort(a,a+k);
        bool ok=0;
        for(int i=s+1;i<=n;i++){
            sum++;
            if(!mp[i]){
                ok=1;
                break;
            }
        }
          bool ok1=0;
            int sum2=0;
            for(int i=s-1;i>=1;i--){
                sum2++;
            if(!mp[i]){
                ok1=1;
                break;
               }
            }
         if(ok1&&!ok)
            cout<<sum2<<endl;
         else if(ok&&!ok1)
            cout<<sum<<endl;
         else if(ok1&&ok)
            cout<<min(sum,sum2)<<endl;
         else if(!ok1&&!ok)
            cout<<0<<endl;
        }
    }
    return 0;
    } 


### B - JOE is on TV! CodeForces - 1293B 
#### 题目大意
s个人，其中n个人答错问题就得到n/s多钱，下一次就只有s-n个人。问理想情况下最多可以得到多少钱？

#### 思路
贪心暴力打表看了一下，就一个人一个人的淘汰才是最优解

    int main(){
    ios::sync_with_stdio(0);
    int n;
    cin>>n;
    double ans=0;
    for(int i=1;i<=n;i++){
        ans+=(double)(1.0/i);
    }
    printf("%lf\n",ans);
    return 0;
    }

### C - NEKO's Maze Game CodeForces - 1293C  技巧高效率
#### 题目大意
一个2 X n 的矩阵,（2<=n<=1e5)，你的任务是从(1,1)走到(1,n),但有些格子会从可以通过变为不可通过，再变化就变为可通过，如此变化，求每次变化是能否完成任务

#### 思路
自己想的时候是，找到第一行的变为不可通过的格子k，然后检查a[i+1][k],a[i+1][k-1],a[i+1][k+1]。这样每次都遍历检查最后TLE test 7了。<br>

正解是：记录不可通过的地方的数目，当为零时，输出yes，否则就no

    int a[3][maxn];
    int main(){
    ios::sync_with_stdio(0);
    int n,q;
    cin>>n>>q;
    mem(a,0);
    int sum=0;
    fro(i,0,q){
        int x,y;
        cin>>x>>y;
        if(!a[x][y]){
            if(x==1)
                sum+=a[x+1][y]+a[x+1][y-1]+a[x+1][y+1];
            else
                sum+=a[x-1][y]+a[x-1][y-1]+a[x-1][y+1];
        }
        else{
            if(x==1)
                sum-=a[x+1][y]+a[x+1][y-1]+a[x+1][y+1];
            else
                sum-=a[x-1][y]+a[x-1][y-1]+a[x-1][y+1];
        }
        a[x][y]==1?a[x][y]=0:a[x][y]=1;
        //cout<<sum<<endl;
        if(sum==0)
            cout<<"Yes"<<endl;
        else
            cout<<"NO"<<endl;
    }
    return 0;
    }


### uva 1615 暴力贪心
#### 题目大意
给定平面上n(n<=1e5)，个点和一个值D，要求x轴上选出尽量少的点，使得给定的点1欧几里得距离不超过D。

#### 思路
按x轴坐标排序，从左到右，贪心间这个点放最边界得出一个点，看其他点是否在这个范围内。

但我又**憨批的没看见多组输入wa了好几次**

    struct node{
    int x,y;
    node(){}
    node(int a,int b):x(a),y(b){}
    bool operator <(const node a)const{
        if(x!=a.x)
        return x<a.x;
        else
        return y>a.y;
    }
    };
    node a[maxn];
    int main(){
    ios::sync_with_stdio(0);
    int len,d,n;
    while(cin>>len){
        mem(a,0);
    cin>>d>>n;
    fro(i,0,n)
    cin>>a[i].x>>a[i].y;
    sort(a,a+n);
    int sum=0,l=0;
    int x;
    x=sqrt(d*d-a[0].y*a[0].y)+a[0].x;
    sum++;
    if(x>=len)
        x=len;
    l++;
    while(l<n){
        if((a[l].x-x)*(a[l].x-x)+a[l].y*a[l].y>d*d){
            x=sqrt(d*d-a[l].y*a[l].y)+a[l].x;
            if(x>=len)
                x=len;
            sum++;
        }
        l++;
    }
    cout<<sum<<endl;
    }
    return 0;
    }