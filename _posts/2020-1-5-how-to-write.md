---
layout: post
title: 扫描法+级角排序+codeforce 612div2
date: 2020-1-5
categories: blog
tags: [计算几何,扫描法,codeforce]
description: 语言
---

## 扫描法
<p style="color: red">类似于一种带有顺序的枚举法，例如：从左到右考虑数组的各个元素，也就是说从左到右扫描。它和普通枚举的重要区别是：扫描法往往枚举时维护一些重要的量。</p>

### Wine trading in Gergovia UVA - 11054 扫描法
### 题目大意
直线上有n个村庄，每个村庄要么买酒，要么卖酒，把k个单位的酒从一个村庄运到相邻村庄需要k个单位的劳动力，问最少需要多少劳动力，使得满足所有村庄的需求。给出n和n个村庄对酒的需求买酒（正数）卖酒（负数）。
### 理解
这道题非常的灵活。。乍一看好像很麻烦，需要搜索，但是实际上对酒的需求量和劳动力是一样的，可以利用这点，从左向右一次扫描每一个村庄，用last表示当前村庄的对酒的需求情况（到最后一个时，last必定为0）然后就相当于他要向下一个村庄转移这么多酒，即需要花费这么多的劳动力。

    int main()
    {
    ios::sync_with_stdio(0);
    int n;
    while(cin>>n)
    {
        if(n==0)
        break;
        ll a,res=0,last=0;
    for(int i=0;i<n;i++)
    {
        cin>>a;
        res+=abs(last);
        last+=a;
    }
    cout<<res<<endl;
    }
    return 0;
    }

### Amphiphilic Carbon Molecules UVA - 1606 级角排序+扫描法
#### 题目大意
平面上有n（n≤1000）个点，每个点为白点或者黑点。 现在需放置一条隔板，使得隔板一侧的白点数加上另一侧的黑点数总数最大。 隔板上的点可以看作是在任意一侧。
#### 理解
视频讲解：<https://www.bilibili.com/video/av29262199><br/>
在理解这道题之前，得理解这几个名词。<br/>
<p style="color: red;font-size: 23px;">相对坐标</p>
即相对于一个同一个点的坐标，方便求叉积，实际上就代表一条直线。
<p style="color: red;font-size: 23px;">级角</p>
极坐标的表示形式。在cmath中有atan2(y,x)可求得级角，相对于atan(x,y)，atan2也被叫做四象限反正切函数，因为它的范围为（-pi,pi)。这道题的扫描法基于级角排序，将n^3的复杂度降到,n^2 logn。
<p style="color: red;font-size: 23px;">扫描</p>
以一条边为基线，根据基点做射线，扫描。

采用极角排序+扫描的方式还需要几个细节问题需要我们去解决<br>

1.如何计算极角<br>

在这里我们需要采用cmath里面的库函数atan2(double y,double x)<br>

atan2(y,x)所表达的意思是以坐标原点为起点，指向(x,y)的射线在坐标平面上与x轴正方向之间的角的角度。也就是当我们枚举到点i时，我们计算出其他点以i为原点后的相对坐标后，带入atan2函数即可求得极角，然后对结构体进行排序即可完成极角排序工作<br>

2.如何快速动态维护最大值<br>

这里我们可以使用一个小技巧，将每一个黑点，都相对当前的枚举点进行中心对称操作，即把相对坐标取反，这样原本在线坐标的黑点就会出现在右边，在右边的黑点就会出现在左边，这样我们只需要统计直线一侧的所有点即可<br>

3.直线如何旋转扫描<br>

我们将原点和极角排序后最小的点作为初始直线，然后将该直线旋转180°，统计那一侧点的数量，也就是说我们需要两个变量L和R，每次从低到高枚举L，即枚举直线，然后枚举R进行统计一侧的点，直到R和L点构成的直线超过180°，对180°的判断我们采用叉积的形式，<br>

<p style="color: red;font-size: 25px;">有个细节需要注意。视频啊婆主讲到了，用atan2排序和用叉积排序的快慢问题。atan2排序慢但精确度高（430ms)，而叉积快但精确度不高(390ms)。这道题的数据范围可能看不出来,但数据范围大了可以考虑使用不同的方法！</p>

    struct point{
    int x,y,color;
    double rad;
    void input(){
        cin>>x>>y>>color;
    }
    bool operator<(const point &b)const{
        return rad<b.rad;
    }
    };
    int cheak(point a,point b){
    return a.x*b.y-a.y*b.x;
    }
    //bool cmp(point a,point b){//级角排序
    //    if(cheak(a,b)==0)
    //        return a.x<b.x;
    //    else
    //        return cheak(a,b)>0;
    //}
    point a[1010],b[1010];
    int main(){
    ios::sync_with_stdio(0);
    int n,maxa;
    while(cin>>n&&n){
         for(int i=0;i<n;i++){
            a[i].input();
         }
         maxa=0;
         for(int i=0;i<n;i++){//基准点
             int k=0;
            for(int j=0;j<n;j++){
                if(i!=j){//不同的两个点
                    b[k].x=a[j].x-a[i].x;//相对坐标
                    b[k].y=a[j].y-a[i].y;
                    if(a[j].color){//中心对称，优化
                        b[k].x=-b[k].x;
                        b[k].y=-b[k].y;
                    }
                    b[k].rad=atan2(b[k].y,b[k].x);
                    k++;
                }
            }
            sort(b,b+k);//级角排序
            //扫描
            int l=0,r=0,sum=2;//l为基准线，r为扫描线,sum为初始两个点
            while(l<k){
                if(l==r){//两个线重合
                    r=(l+1)%k;
                    sum=2;
                }
                while(l!=r&&cheak(b[l],b[r])>=0){//叉积判断是否大于180度
                    r=(r+1)%k;
                    sum++;//旋转加一
                }
                maxa=max(maxa,sum);
                sum--;//基准线变化后，基点不变，另外一个点减掉
                l++;
            }
         }
         cout<<maxa<<endl;
    }
    return 0;
    }



## codeforce 612div2
### Angry Students CodeForces - 1287A 模拟
#### 题目大意
一个序列由P和A两种字符组成，一分钟，A就可以把他前面的P变为A，若A前面没有字符就不变，问多少分钟后序列全为A。
#### 理解
暴力模拟即可

    int main(){
    ios::sync_with_stdio(0);
    int t;
    cin>>t;
    while(t--){
        int k;
        string s;
        cin>>k;
        cin>>s;
        bool ok=0;
        int sum=0;
        while(1){
                ok=0;
            for(int i=0;i<k;i++){
                if(s[i]=='A'&&i+1!=k&&s[i+1]=='P'){
                    s[i+1]='A';
                    i++;
                    ok=1;
                }
            }
            if(!ok)
             break;
                sum++;
        }
        cout<<sum<<endl;
    }
    return 0;
    }

### Hyperset CodeForces - 1287B 暴力+构造 高效算法设计
#### 题目大意
n个序列，长度都为k，任选三个序列满足序列对应位置要么全部相同，要不两两不同？？？（我就是这里不太懂？？），序列由‘S' 'E' 'T'三种字符组成。n<=1500
#### 题解
害！真的不是很懂两两不同，最后看别人代码才晓得。只需考虑对用位置要么全部相同，要不全部不同，（我啥子英语？？）。暴力三个会T。所以可以枚举两个序列，构造出另外一个，用map去查找序列种是否存在这样的字符串，复杂度n^2longn。

    string s[2020];
    map<string,int> mp;
    char ss[3]={'S','E','T'};
    int main(){//构造
    ios::sync_with_stdio(0);
    int n,m;
    cin>>n>>m;
    fro(i,0,n){
        cin>>s[i];
        mp[s[i]]++;
    }
    int ans=0;
    fro(i,0,n){
        for(int j=i+1;j<n;j++){
            string gz="";
                //cout<<s[i]<<" "<<s[j]<<endl;
            for(int k=0;k<m;k++){
                if(s[i][k]==s[j][k]) gz+=s[i][k];
                else{
                bool book[4]={0};
                fro(l,0,3){
                    if(s[i][k]==ss[l])book[l]=1;
                    if(s[j][k]==ss[l])book[l]=1;
                    if(!book[l])gz+=ss[l];
                }
                }
            }
          //  cout<<gz<<endl;
            ans+=mp[gz];
        }
    }
    cout<<ans/3<<endl;
    return 0;
    }